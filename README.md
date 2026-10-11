# qresev-public

> The redacted public slice of the **financial domain** behind the
> **Qresev** product: a Lean4 axiomatic evaluator for stocks and
> portfolios, plus the market-inspection and portfolio-management
> engineering that feeds it. Windows three private subprojects —
> `accounting/` (the formal kernel), `analyzing/` (market inspection),
> and `trading/` (portfolio-management agents).

A window into the formal-financial kernel and its supporting engineering,
running alongside the private working repository. It carries prose only — no
kernel source is published here. The kernel shares the same three-layer split as
[`qnarre-public`](https://github.com/quantapix/qnarre-public),
different domain: instead of complaints under federal statutes, it
proves portfolio-level judgments over OHLCV bars + indicator series +
GICS sector mappings.

- Parent organisation: <https://github.com/quantapix>
- Engineering output: <https://quantapix.com>
- Live product: <https://qresev.quantapix.com>

## What this repo windows

Three private subprojects feed one public-facing financial slice. They
share a domain (equities + defined-risk options) but **no code** — each
reads shared data through thin loaders, never imports another's modules.

| Surface | Private subproject | Role |
|---|---|---|
| **Formal evaluator** | `accounting/` | Lean4 kernel + LLM-backed predicates. The verifier that backs Qresev. |
| **Market inspection** | `analyzing/` | A local-only TypeScript viewer plus Python ingest scripts over a local OHLCV store — ingest, indicator reference, charting. The data layer the kernel's evidence is drawn from. |
| **Portfolio management** | `trading/` | Three portfolio-management agents on a paper broker, each with its own capital, strategy, and journal. Their scheduled routines have been switched off since 2026-09-20. The decision layer whose tickets the kernel can gate. |

The pipeline reads left-to-right: `analyzing/` produces the bars and
indicator reference; `trading/` proposes positions; `accounting/`
proves whether a portfolio (or a single candidate) satisfies a named
framework. The product's API replays a committed example through the kernel
and streams the result; the app page does not consume that stream yet
([`STATUS.md`](./STATUS.md) says exactly what it shows today).

---

## 1. The formal evaluator (`accounting/`)

### The three-layer split

| Layer | Reads | Writes | Tools |
|---|---|---|---|
| **Formal kernel** (`Accounting/<Framework>/*.lean`) | only Lean | only Lean | Lean elaborator. No I/O, no LLM calls. |
| **Predicate functions** (`predicates/<framework>/*.md`) | one portfolio JSON + bar/indicator evidence | a single `Bool` (plus evidence + uncertainty) | LLM sub-agent with `context: fork`. |
| **Driver** (`scripts/extract_facts.py`) | manifest + portfolio | the framework's generated axiom block + audit JSON | LLM invocations, top-tier model only. |

The Lean kernel never reads OHLCV bars, indicator series, parquet, or
JSON. The predicate sub-agents never write Lean. The driver is a thin
coordinator with no financial reasoning of its own. The verifiable
proof IS the Lean elaboration trace produced by `lake build`.

The driver runs every committed extraction on a top-tier model; a
bake-off showed weaker models fabricate numeric evidence while reporting it
confidently, so a cheaper tier is refused for any retained run. An
anti-poison guard rejects and retries evidence that carries no numbers.

### Frameworks

Ten framework directories carry kernels and judgments today. Five of them —
Trend, Momentum, OptionsRisk, Sector, Drawdown — are the **golden reference**:
hand-built, worked end-to-end on committed example portfolios, and the set the
automated program is scored against. Three more landed under a chartered
expansion. Two are *program* frameworks rather than portfolio judgments: the
bull-vs-bear gate, and a factor decomposition that ships as its own kernel
library.

| Framework | Top-level judgment | Scope |
|---|---|---|
| **Trend** | `is_uptrend`, `is_downtrend` | per portfolio |
| **Momentum** | `has_momentum` | per portfolio |
| **OptionsRisk** | `defined_risk_only`, `is_clean` | per book |
| **Sector** | `has_violation`, `no_violations` | per book |
| **Drawdown** | `drawdown_disciplined` | per book |
| **Regime** | `regime_trending`, `regime_ranging`, `regime_high_vol`, `regime_compressed` | per symbol, non-exclusive preconditions |
| **ExitDiscipline** | `exit_disciplined` | per position |
| **Hedging** | `hedge_disciplined`, `hedge_regime_consistent` | per book |
| **Debate** | `admissible_long` | per candidate |
| **Factor** | `factor_disciplined` | per factor-decomposition subject |

Two shapes in the newer three are worth naming. **Regime** judgments are
non-exclusive preconditions rather than verdicts — a symbol may satisfy several
at once — and each carries an explicit refutation dual, so "not established" and
"established false" are different kernel facts. **ExitDiscipline** is the first
per-*position* framework: four of its prongs are required *negations* (a fired
exit signal disqualifies), so the composite cannot be constructed while a
disqualifier holds.

The **Hedging** framework's two judgments landed as whole structures with no
omittable field: a sizing-and-volatility-and-floor conjunction, and a consistency
judgment tying a hedge's presence to the prevailing regime. Its last three
predicates had been recorded as blocked rather than stubbed, because they need
premium history the portfolio schema did not carry. They landed on 2026-08-22
once a fills ledger existed to read: they take entry facts from that ledger and
abstain when a position carries none.

The kernel's predicate names are the contract the Qresev report surfaces
consume. They do not get renamed without lockstep UI edits.

#### Sector caps are a policy, not a constant

The `Sector` framework's concentration cap is parametrised over a **cap
policy** — a function from GICS sector to a weight ceiling — rather than a
single hard-coded number. The default policy applies a uniform ceiling across
all eleven sectors and is what the product's proof trace consumes; a portfolio
with a tighter house limit (or a per-sector tightening, e.g. a lower ceiling on
a single sector) supplies its own policy and the same intro theorems compose
against it. The default specialisation is definitionally equal to the old
fixed-ceiling form, so the kernel's public judgment surface is unchanged.

The **drawdown** bound is deliberately *not* a policy. Maximum-drawdown
discipline is a single universal defined-risk veto applied identically to every
sector. Loosening it per sector would let a name clear the top confluence tier
on relaxed risk terms, which would corrupt the cross-axis agreement signal the
kernel exists to measure. A sector-wide drawdown veto firing is a correct
outcome, not a gap.

### Defined-risk options — hard refusal at the boundary

The `OptionsRisk` framework enforces a six-strategy allow-list:

- `long_call`, `long_put`
- `debit_spread_call`, `debit_spread_put`
- `covered_call`, `protective_put`

Anything outside that set — naked shorts, undefined-risk multi-leg
combos, leveraged structures — is refused at three surfaces:

1. **Authoring** — a skill gates the writing of any options code in
   the `trading/` surface.
2. **Runtime** — a portfolio-management agent's ticket for a disallowed
   structure is rejected before it reaches the broker.
3. **Kernel** — `OptionsRisk.is_clean` requires `defined_risk_only` as
   a precondition; the theorem cannot elaborate against a portfolio
   that violates the allow-list. The kernel encodes the restriction at
   the type level via a closed `Strategy` enum.

This is a load-bearing project rule, not a tunable.

#### The composition question, and why it is not a widening

A structure with a name outside the six — a collar, say — raised a real
question: does supporting it widen the allow-list? The answer settled as a
two-clause test, and it deliberately does not touch the enum. A composed
multi-leg structure is admissible **iff** every leg is independently
allow-listed **and** every short leg is covered by a position the same book
already holds. A collar decomposes into a covered call plus a protective put
and needs no new `Strategy` constructor; a short call with a long put and no
underlying is a synthetic short and stays refused. The allow-list did not get
longer — the composition rule made it possible to say *why* one three-leg
structure is inside it and another is not.

The load-bearing half is the second clause, because "covered" has to be a
fact rather than an inference. A leg-pairing identifier is now minted when the
ticket is written and travels onto every position and ledger row it opens; a
pairing reconstructed afterwards from "same underlying, same day, opposite
sides" is a guess, and a wrong guess reads as cleared without having been
cleared. It also has to survive a *partial* close, or a half-closed collar
reads as an uncovered short. Positions that predate the identifier are charged
the full share requirement rather than credited a spread on inference —
releasing a coverage requirement on a guess is the one direction the rule
forbids.

#### Hierarchical predicates — leaves the model decides, composites the kernel derives

A top-level framework judgment can be decomposed: the model-backed predicates
are pushed down to small, independently-decidable **leaves**, and the composite
that joins them is **derived in the kernel** rather than asserted. As of
2026-08-16 decomposition covers every judgment framework, in three kernel
shapes:

- **Bool-composite.** Boolean leaves closed by a shared tactic — a disjunction
  over routes (the momentum MACD cross, the trend SMA cross) or a
  conjunction-with-required-negation (trend volume confirmation), where a bear
  money-flow guard is a structure *field*, not a vote: the composite cannot be
  built while the disqualifier holds.
- **Measurement-leaf.** A single leaf measures an integer magnitude and the
  kernel settles the bound directly (`decide` / `omega`), with no model call in
  the loop. This now covers all three bound directions — an upper bound
  (maximum drawdown against its ceiling, sector concentration against its cap),
  a lower bound (a trend fit quality floor), and a two-sided band (a momentum
  oscillator range). Fractions lift to basis points, bounded indices to
  centi-units, so the comparison is exact integer arithmetic.
- **Live-judgment landings.** Several decomposed elements now compose back into
  the live top-level judgment via a kernel composition lemma, not just as
  standalone atoms.

The flat golden judgments are unchanged — decomposition is an
additive, more granular proof of the same conclusion.

### Factor decomposition — a kernel judgment with no model in the loop

The newest framework has a property none of the others do: **it makes no LLM
calls at all.** A separate fit lane derives a small set of per-symbol factor
return series over the intersection of the bar store, the indicator reference
and the sector map, with a formation lag, leave-one-out sector baskets, and
split-integrity re-derivations of the intermediate columns. The kernel then
checks one thing about the result — the exact quantize-then-residualize
identity, as an integer fold closed by `decide` — plus residual and
out-of-sample bounds. The facts are lifted from the fit report by a
stdlib-only emitter that is *statically* free of model calls and fails closed
if the report's content hash does not match what it was told to read.

Three properties are worth naming because each one closes a way the
decomposition could have lied:

- **The identity refuses rather than approximates.** A vector-length
  mismatch, a day-count mismatch, or an empty sample all evaluate to `false`.
  There is no tolerance parameter, and a decomposition that does not
  reconstruct its own inputs does not elaborate.
- **A fit report influenced by the graph surface is refused outright** at the
  emitter, before it can become an axiom. The decomposition exists to be an
  independent reading; letting the rendering layer feed back into it would make
  it a second opinion about the first opinion.
- **The judgments are new and parallel, never a widening of an existing
  one.** `factor_disciplined` composes alongside the golden frameworks and
  never re-targets a golden bridge, so adding it cannot move a conclusion that
  was already proved.

Promotion of a decomposition set is session-fired and never automatic, the
promoted object is the whole per-date set rather than a file, and a restatement
of an already-promoted set is refused unless explicitly forced. One further
subproject is licensed to read the promoted copies as a second reader — named
explicitly, additively, and read-only; it has no write path back, and it is
barred from the kernel and from every agreement quorum, so a simulation can
never become evidence for a proof.

### Predicate spec shape

Each predicate is one markdown file. Its frontmatter names the predicate,
describes it in a sentence, declares that it runs in a forked context, and lists
the tools the sub-agent may use. The body has six required sections:

1. **Lean signature** — the exact `axiom` declaration the predicate discharges.
2. **Inputs** — the entity references the sub-agent expects alongside the
   portfolio.
3. **Authority** — the indicator definition and threshold rationale (a column
   of the committed indicator reference, with TA-Lib parity where TA-Lib
   defines the indicator) and, for the options framework, the allow-list rule.
4. **Decision rubric** — a numbered list of conditions; the output is `true`
   iff a documented threshold is met.
5. **Output schema** — `{ value: Bool, evidence: [...], uncertainty }`, where
   `uncertainty` is a level, not a number.
6. **Test cases** — at least one positive and one negative.

The driver records each returned value as a Lean `axiom` in the framework's
generated facts file. An earlier version of this section showed a frontmatter
block with fields the specs do not carry.

### Build and verify

In the private tree, verification is one command:

```
lake build
```

Either the kernel elaborates against the generated facts — the
portfolio judgment holds across every framework its manifest names — or it does not,
in which case the failing theorem names the predicate that does not
provide enough evidence under the current axiom set. There is no "sort
of holds."

### Worked examples

In the private tree, `examples/<id>/` holds per-portfolio artifacts: the
synthetic portfolio, the run manifest, the per-predicate facts, the generated
axiom block, the proof file, and the report, proof-graph and failure-loci
files the product surfaces read. The brand-anchor example exercises four of
the five golden frameworks.

The accept path of the drawdown framework is now exercised directly: a
conservative cash-heavy example portfolio elaborates
`drawdown_disciplined` alongside an options book that is clean vacuously (it
has no legs) and a sector-cap proof under a tightened house policy.

---

## 2. Market inspection (`analyzing/`)

`analyzing/` has two roles behind the financial slice.

**As market-data supplier** it is the named producer of the historical
aggregate **tape** — the OHLCV bars and the TA-Lib-parity indicator reference —
over the S&P 500 plus anchor-ETF universe, on locked schemas. This is the
ground truth the `accounting/` kernel's evidence is drawn from: the kernel
reads the tape as files and never imports the analyzer, and the portfolio
agents consult it for decision support only — the live order path never touches
the tape.

**As a viewing surface** it re-homes market inspection as a local-only browser
surface (never publicly deployed) that mounts the shared graphing kits and
drives a sector ▸ industry ▸ symbol (and portfolio ▸ holdings) drill-down over
the same ground-truth store. It is run-independent, ground-truth-only browsing —
the complement to the run-keyed product surface.

Two invariants hold the seam clean: the consumer reads **files, never a served
API**, and the viewer renders **ground-truth tape only, never kernel
emissions** — no predicate verdict, framework judgment, or proof node appears in
an inspection panel. GICS mappings and portfolio snapshots are read-only here;
the analyzer never writes them and never becomes an order surface.

- **Storage.** A columnar local store (Parquet, queried through an
  embedded analytical SQL engine) keyed by symbol. No vendor lock-in;
  ingest is from public market-data sources.
- **Canonical bar shape.** Every bar carries the same column set:
  `{ ts, o, h, l, c, v, adj_c }`. Vendor field names die at the
  boundary — adapters translate at the client module; the kernel never
  sees vendor strings. (See the cross-domain bar-shape note below.)
- **TA-Lib parity.** Indicator math matches TA-Lib semantics exactly,
  including the MACD EMA-realignment quirk where both EMAs align to the
  slow-period start. A Python TA-Lib reference is the ground truth that
  indicator code is checked against.
- **Universe.** A committed list pins the S&P 500 constituents plus
  anchor ETFs (sector and broad-market); a batch ingest chunks the fetch and isolates
  per-symbol failures so a full-universe refresh never rides one giant
  download.
- **A refresh is one ordered chain, not one command.** Bars, then the
  indicator reference, then the materialised market-breadth series, then the
  smaller index and sub-daily lanes, and last the freshness manifest that
  cross-checks them. The breadth lane carries a single implementation of its
  recurrences, and the manifest refuses to call the tape coherent if the daily
  bars, the indicator reference and the breadth series disagree on their as-of
  date.
- **A pinned tape of record.** A committed pin names the content hash of the
  tape a checkout is supposed to hold, so a kernel run on a lagging machine
  aborts instead of quietly proving something against older data. The pin moves
  forward only, and never from a machine that is itself behind.

The indicator reference produced here is exactly what the
`accounting/` `Trend` / `Momentum` predicates consult, and what the
`trading/` debate (below) scores confluence against. The seam is a
shared data file, not a code import.

---

## 3. Portfolio management (`trading/`)

`trading/` holds three portfolio-management agents
(aggressive / moderate / conservative) on a paper-trading broker. Each
starts with the same notional capital and aims to beat the benchmark
on a monthly basis. Their scheduled routines have been switched off, by
operator decision, since 2026-09-20; they can still be run by hand. The discipline that governs them is the part worth
publishing — the agents themselves are private.

### Hard rules (the discipline)

1. **Paper only.** Every broker call routes through the paper endpoint.
2. **Defined-risk options only.** The same six-strategy allow-list the
   kernel enforces; no structure whose max loss cannot be computed at
   trade time.
3. **Not day trading.** Holding horizons are days to months. The midday
   routine biases toward doing nothing; it acts only on a material new
   event.
4. **Risk gate before execution.** Every equity ticket clears a risk
   analyzer; every options ticket clears the options-risk analyzer *and*
   the risk analyzer, before an executor places it.
5. **Separate capital per agent.** The three agents never share
   positions, cash, or broker accounts. Each has its own key-pair,
   portfolio, strategy doc, and journal.
6. **Journal before quitting.** A routine that places or modifies a
   position appends its reasoning to a dated journal before exiting; if
   journaling fails, the run is failed.
7. **Stop-losses on every equity buy, and coverage is a claim about the
   broker.** What the agent *intended* is recorded on the position; whether a
   protective order is actually resting is a separate question answered by
   querying the broker, and the two are never conflated. A verification that
   cannot see the whole order tree returns *unknown* rather than a breach.
8. **The risk policy changes only at the monthly retrospective,** with the
   change justified in writing.
9. **Concentration caps gate a proposed ticket.** A held position can drift
   through its cap on price alone, so the close re-validates the book and
   reports a breach for the next session — it reports, it does not cure.

That seventh rule is stated in that shape because both halves of it were
learned the expensive way, and the honest version of the story is in
[`STATUS.md`](./STATUS.md).

### Bull-vs-bear debate — gated by the kernel

Any agent can run a parallel **bull vs bear** AI-session debate on a
candidate symbol, then **hard-gate** the verdict through the
`accounting/` Lean kernel before it becomes a ticket. The gate is a
**veto, not a vote**: a passing verdict still has to clear the risk
analyzers before it can be placed. Evidence is already-reported
data / news / trends plus the indicator reference from `analyzing/` —
no new data source. Gate coverage today is the `Trend.is_uptrend`
judgment over the analyzing-side S&P 500 universe; it abstains on names
outside it.

This is where the three subprojects meet: `analyzing/` supplies the
evidence, `trading/` proposes the position and argues both sides, and
`accounting/` proves whether the long is admissible. The
**Debate** kernel framework formalizes the gate itself — `admissible_long`
is a structure with no constructor omitting any field, so it requires
the bull confluence claim, the kernel directional accept, a
defined-risk-clean check, and the negation of every bear disqualifier.
"Veto, not a vote" becomes a type-level property.

---

## Cross-domain conventions

These hold across all three surfaces (and are shared with the broader
framework — see [`qagents-public`](https://github.com/quantapix/qagents-public)).

### Canonical OHLCV bar shape

Every place that produces or consumes bar data uses the same column
set: `{ ts, o, h, l, c, v, adj_c }`. No `t` instead of `ts`, no missing
`adj_c`, no vendor renames past the client module.

### GICS sector classification

Mapping is **shared data, not shared code**: a Parquet file keyed by
symbol carries the canonical GICS hierarchy. The eleven GICS long-form
sector names are the canonical strings; portfolio forms accept nothing else.
Both the analyzer and the kernel read it through thin loaders; neither
writes it. A sector-cap claim like "max 25% Information Technology" is a
Lean `Prop` with a structured witness, not a spreadsheet count.

### Language split

TypeScript for the market-inspection viewer, with Python for its ingest and
tape scripts; Python for the portfolio agents and the kernel driver; Lean4 for the formal kernel. A
Python microservice is allowed as a numerics escape hatch (the TA-Lib
reference). The surfaces never reach across: the trading Python does
not import the analyzing TypeScript, and neither imports the kernel's
Lean.

---

## Axiomatize-trading program

The five hand-built frameworks are the **golden reference** for a larger
program: a kernel-checked encoding of trading signals across the whole
S&P 500 universe, produced redundantly by independent agent teams along
several orthogonal **signal-family** axes, then reconciled. Cross-axis
agreement, checked in the kernel by bridge lemmas that discharge under declared
correspondences, is the correctness signal — and it coincides with the practitioner's **confluence** heuristic
(independent indicator families agreeing). The slice unit is the
`(GICS sector, axis)` cell. This mirrors the full-U.S.-Code program on the
legal side ([`qnarre-public`](https://github.com/quantapix/qnarre-public)).

**Tier language on this page is withdrawn, and the re-verification that would
have restored it did not.** Cross-axis *agreement* is what promotes a name to
the top confluence tier, and in July 2026 our own adversarial review lane found
leak channels in the agreement oracle. A re-slice of the names still
eligible on that tape then completed and **reproduced no top-tier result**.
Every agreement-derived tier claim below is dated pre-correction and remains
withdrawn; the mechanical facts — what is encoded, and whether it elaborates
`sorry`-free — are unaffected. [`STATUS.md`](./STATUS.md) carries the correction
and the current posture, and the public status board's *withheld* outranks any
number on this page.

The GICS universe is fully enumerated (the full eleven-sector membership),
and the program runs end-to-end through promotion: the shared predicate
library holds 34 predicates, unchanged since June 2026 — among them a
structural options-book cluster (per-leg allow-list, naked-short exclusion,
debit-only, bounded max loss) — whose collapse bridges were produced by
pre-correction waves, and the first per-sector slice
(Real Estate) is committed into the kernel library as an explicit build
root. The default run-set carries seven axes (five core signal families
plus instrument-risk and tradability/liquidity). Single-sector waves have so
far always landed below the top confluence tier: only a subset of the axes are
directional, and the cross-section axis is treated as provisional until a
cross-sector pass.

A sixth **Debate** framework formalizes the bull-vs-bear gate itself.
`admissible_long` is a structure with no constructor omitting any field, so
it requires the bull confluence claim, the kernel directional accept, a
defined-risk-clean check, and the negation of *every* bear disqualifier.
"Veto, not a vote" becomes a type-level property. Its first kernel runs
**refuse-closed** inadmissible candidates (no `sorry`, `allowed = false`), on a
strong veto in one case and a weak one in another. A weak veto still blocks.
Those runs predate the July correction; an earlier version of this paragraph
described the refused candidates closely enough to identify them, and that
description has been removed.

Targeting a wave is now a deterministic three-stage filter that runs before any
kernel or model work — pure indicator and relative-strength arithmetic over the
full symbol store:

1. **Sector rotation.** Rank the eleven sector ETFs by relative strength against
   the benchmark. A new-sector top-tier result is reachable only where that
   sector's ETF is leading; rotation, not risk, is usually the binding
   constraint.
2. **Confluence pre-screen.** Over the survivors, find the names where the
   independent trend and momentum families agree.
3. **Volatility veto.** Apply the defined-risk volatility gate (a maximum-
   drawdown ceiling and an average-true-range bound). This is the actual
   top-tier gate — confluence alone does not promote a name when the veto fires.

A standing frontier view joins all three stages into one watch-list, labelling
each candidate by its binding gate (eligible / near-miss / laggard-gated /
rotation-gated / veto-killed / promoted).

As of 2026-10-04 the program has encoded symbols in **five** sectors — real
estate, industrials, utilities, financials and health care. Counting only
committed kernel encodings (the golden calibration portfolios are scaffolding,
not universe members), the encoded share of the ~526-symbol universe is about
**2%**, none depending on `sorry` at the last recorded check.

The newest sector landing, on 2026-08-22, is a worked instance of the
discipline, not a call on any name. It cleared the deterministic pre-filter and
was still held one tier below the top, because its directional bridges were
conditioned on evidence gaps. Clearing the filter is necessary and not
sufficient; a refusal-to-promote is a correct outcome, not a gap. The
full-universe scale-out remains gated on a separate programmatic lane, which is
parked with no planned resumption date.

The 2026-07-31 multi-sector wave illustrates the gate rather than the
numerator. It ran the full pipeline across four sectors, every cell elaborated,
every gate that had something to check passed — and **nothing was promoted.**
Its blindness certification came back a *breach* verdict and was frozen as a
breach rather than argued down. The finding at the time covered the shared
kernel file, which only the licensed reconciler touched. A second channel found
later did reach the blind cells: the per-axis briefings every cell is required
to read named the reference judgments. Those briefings have been scrubbed, the
scan that missed them now reads every required document, and the wave stays
unpromoted. Freezing an inconvenient verdict before reacting to it is the only
order of operations that keeps a certification meaningful; a wave whose
certificate was adjusted after the fact would be evidence of nothing.

## What this repo is not

- **Not financial advice.** The evaluator checks a portfolio's
  predicate structure against named frameworks; it does not recommend
  trades, position sizes, or strategies. The portfolio agents trade
  paper only.
- **Not a chat surface.** Predicate sub-agents are scoped, audited, and
  short-lived.
- **Not a private-PII surface.** The hosted service accepts no user
  portfolio at all today: it replays three committed, synthetic examples. No
  real account numbers, real names, or private metadata flow to the server or
  to any LLM.

## Cadence

Refreshed from the private working tree, and only when a financial sign-off
clears the new text — so this repository can lag the weekly cadence of its
siblings, and between August and October 2026 it did. Each refresh is an
ordinary diff; the commit log is the change record. See [`STATUS.md`](./STATUS.md) for the current
release-narrative snapshot.

Authored by a sole developer working with an AI assistant (Claude Code) under written CLAUDE.md contracts — methodology in [qagents-public](https://github.com/quantapix/qagents-public).

## Contact

[github.com/quantapix](https://github.com/quantapix) — open an issue on any repo
in the org. Answered in public; there is no contact email.

## License

Apache-2.0 (`LICENSE.txt`). Code-class repo — Lean axioms, predicate
stubs, drivers, and the engineering-discipline prose are reused under
the standard Apache patent grant.
