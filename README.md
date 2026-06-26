# qresev-public

> The redacted public slice of the **financial domain** behind the
> **Qresev** product: a Lean4 axiomatic evaluator for stocks and
> portfolios, plus the market-inspection and portfolio-management
> engineering that feeds it. Windows three private subprojects —
> `accounting/` (the formal kernel), `analyzing/` (market inspection),
> and `trading/` (portfolio-management agents).

A weekly-refreshed window into the formal-financial kernel and its
supporting engineering, running alongside the private working
repository. The kernel shares the same three-layer split as
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
| **Market inspection** | `analyzing/` | A TypeScript editor extension over a local OHLCV store — ingest, indicators, charting. The data layer the kernel's evidence is drawn from. |
| **Portfolio management** | `trading/` | Three autonomous portfolio-management agents on a paper broker, each with its own capital, strategy, and journal. The decision layer whose tickets the kernel can gate. |

The pipeline reads left-to-right: `analyzing/` produces the bars and
indicator reference; `trading/` proposes positions; `accounting/`
proves whether a portfolio (or a single candidate) satisfies a named
framework. The product surface streams the kernel's verdict.

---

## 1. The formal evaluator (`accounting/`)

### The three-layer split

| Layer | Reads | Writes | Tools |
|---|---|---|---|
| **Formal kernel** (`Accounting/<Framework>/*.lean`) | only Lean | only Lean | Lean elaborator. No I/O, no LLM calls. |
| **Predicate functions** (`predicates/<framework>/*.md`) | one portfolio JSON + bar/indicator evidence | a single `Bool` (plus evidence + uncertainty) | LLM sub-agent with `context: fork`. |
| **Driver** (`scripts/extract_facts.py`) | manifest + portfolio | the framework's generated axiom block + audit JSON | LLM invocations, Opus only. |

The Lean kernel never reads OHLCV bars, indicator series, parquet, or
JSON. The predicate sub-agents never write Lean. The driver is a thin
coordinator with no financial reasoning of its own. The verifiable
proof IS the Lean elaboration trace produced by `lake build`.

The driver runs every committed extraction on a frontier model (Opus); a
bake-off showed weaker models fabricate numeric evidence while reporting it
confidently, so a cheaper tier is refused for any retained run. An
anti-poison guard rejects and retries evidence that carries no numbers.

### Frameworks

Five frameworks, each under `Accounting/<Framework>/` (kernel) +
`predicates/<framework>/` (specs).

| Framework | Kernel | Top-level judgment | Predicates |
|---|---|---|---:|
| **Trend** | `Accounting/Trend/` | `is_uptrend`, `is_downtrend` | 5 |
| **Momentum** | `Accounting/Momentum/` | `has_momentum` | 4 |
| **OptionsRisk** | `Accounting/OptionsRisk/` | `defined_risk_only`, `is_clean` | 6 |
| **Sector** | `Accounting/Sector/` | `has_violation`, `no_violations` | 3 |
| **Drawdown** | `Accounting/Drawdown/` | `drawdown_disciplined` | 3 |

The kernel's predicate names are the contract the Qresev `/app` Lean
trace consumes. They do not get renamed without lockstep UI edits.

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

#### Hierarchical predicates — leaves the model decides, composites the kernel derives

A top-level framework judgment can be decomposed: the LLM-backed predicates are pushed down to small, independently-decidable **leaves**, and the composite that joins them is **derived in the kernel** rather than asserted. Numeric comparisons — a moving-average cross, a drawdown magnitude against its ceiling — lift to integer arithmetic the Lean elaborator settles directly (`decide` / `omega`), with no model call in the loop. Three frameworks are decomposed this way today: the momentum MACD cross (a disjunction over its component leaves), the trend SMA cross (five moving-average leaves over two routes), and the maximum-drawdown veto (a single measured magnitude bounded against its ceiling). The flat golden judgments are byte-for-byte unchanged — decomposition is an additive, more granular proof of the same conclusion, and the goldens still elaborate identically.

### Predicate spec shape

```yaml
---
predicate: <name>
framework: trend | momentum | options-risk | sector | drawdown
returns: Bool
inputs:
  portfolio: <slug>
  bars: <date range>
evidence_required: <one-line description>
uncertainty_cap: 0.20
---
```

Body sections: **Question**, **Authority** (the indicator-library
reference; TA-Lib parity is the contract for indicator predicates),
**Evidence**, **Adversary case**, **Output**. The sub-agent returns a
JSON object with `{ value: Bool, evidence: [...], uncertainty: [0,1) }`.
The driver records each as a Lean `axiom` in the framework's generated
facts file.

### Build and verify

```
lake build
```

Either the kernel elaborates against the generated facts — the
portfolio judgment holds across all five frameworks — or it does not,
in which case the failing theorem names the predicate that does not
provide enough evidence under the current axiom set. There is no "sort
of holds."

### Worked examples

`examples/<id>/` holds per-portfolio artifacts: the redacted portfolio
JSON, the bar evidence view, the per-predicate audit JSON, the
generated axiom block, the driver's audit log, and the Lean
elaboration trace. The brand-anchor example exercises all five
frameworks across six judgments.

The accept path of the drawdown framework is now exercised directly: a
conservative cash-heavy example portfolio elaborates
`drawdown_disciplined` alongside a clean options book and a sector-cap
proof under a tightened house policy.

---

## 2. Market inspection (`analyzing/`)

`analyzing/` is the data layer: a TypeScript editor extension that
ingests public OHLCV bars, computes a TA-Lib-parity indicator
reference, and charts both. It is the source the kernel's evidence is
drawn from — predicate sub-agents read bars and indicators that this
surface produced.

- **Storage.** A columnar local store (Parquet, queried through an
  embedded analytical SQL engine) keyed by symbol. No vendor lock-in;
  ingest is from public market-data sources.
- **Canonical bar shape.** Every bar carries the same column set:
  `{ ts, o, h, l, c, v, adj_c }`. Vendor field names die at the
  boundary — adapters translate at the client module; the kernel never
  sees vendor strings. (See the cross-domain bar-shape note below.)
- **TA-Lib parity.** Indicator math matches TA-Lib semantics exactly,
  including the MACD EMA-realignment quirk where both EMAs align to the
  slow-period start. A Python TA-Lib reference is the ground truth; the
  parity check runs on every framework that touches indicators.
- **Universe.** A committed list pins the S&P 500 constituents plus
  anchor sector ETFs; a batch ingest chunks the fetch and isolates
  per-symbol failures so a full-universe refresh never rides one giant
  download.

The indicator reference produced here is exactly what the
`accounting/` `Trend` / `Momentum` predicates consult, and what the
`trading/` debate (below) scores confluence against. The seam is a
shared data file, not a code import.

---

## 3. Portfolio management (`trading/`)

`trading/` runs three autonomous portfolio-management agents
(aggressive / moderate / conservative) on a paper-trading broker. Each
starts with the same notional capital and aims to beat the benchmark
on a monthly basis. The discipline that governs them is the part worth
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
sector names are the only accepted strings; abbreviations do not parse.
Both the analyzer and the kernel read it through thin loaders; neither
writes it. A sector-cap claim like "max 25% Information Technology" is a
Lean `Prop` with a structured witness, not a spreadsheet count.

### Language split

TypeScript for the market-inspection extension; Python for the
portfolio agents and the kernel driver; Lean4 for the formal kernel. A
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
agreement, proved in the kernel via Bridge lemmas, is the correctness
signal — and it coincides with the practitioner's **confluence** heuristic
(independent indicator families agreeing). The slice unit is the
`(GICS sector, axis)` cell. This mirrors the full-U.S.-Code program on the
legal side ([`qnarre-public`](https://github.com/quantapix/qnarre-public)).

The GICS universe is fully enumerated (the full eleven-sector membership),
and the program now runs end-to-end through promotion: the shared predicate
library has grown wave-over-wave to 34 collapse-bridged predicates — among
them a complete structural options-book cluster (per-leg allow-list,
naked-short exclusion, debit-only, bounded max loss) proved equivalent
across sectors under sorry-free bridges — and the first per-sector slice
(Real Estate) is committed into the kernel library as an explicit build
root. The default run-set carries seven axes (five core signal families
plus instrument-risk and tradability/liquidity). A structural invariant
held exactly as designed: a single-sector wave can never reach the top
confluence tier, because only a subset of the axes are directional and the
cross-section axis stays provisional until a cross-sector pass — the first
top-tier promotion waited until a cross-sector reconciliation confirmed
sector leadership as the third independent directional axis.

A sixth **Debate** framework formalizes the bull-vs-bear gate itself.
`admissible_long` is a structure with no constructor omitting any field, so
it requires the bull confluence claim, the kernel directional accept, a
defined-risk-clean check, and the negation of *every* bear disqualifier.
"Veto, not a vote" becomes a type-level property. The first kernel run of it
correctly **refuse-closed** an inadmissible candidate (no `sorry`,
`allowed = false`). The first **accepting** run followed within the week: a
real-estate REIT (FRT) cleared all three directional axes — trend, momentum,
and cross-section leadership against the benchmark — with zero bear vetoes,
and `admissible_long` elaborated `allowed = true` with no `sorry`. In the
same wave the strongest-trending candidate on the board, a semiconductor
name (NXPI), was refuse-closed by the volatility veto on a ~47% historical
drawdown; a later wave refuse-closed a hotel REIT (HST) on a *weak*
volume-divergence veto. A weak veto still blocks — veto, not a vote,
exercised on both branches.

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
each candidate by its binding gate (eligible / near-miss / rotation-gated /
veto-killed / promoted).

The program now has promoted citizens across three sectors — real estate,
industrials, and utilities — each reconciled sorry-free. Counting only committed
kernel encodings (the golden calibration portfolios are scaffolding, not universe
members), the encoded share of the ~526-symbol universe is on the order of 1.7%,
every encoding sorry-free. The structural invariant holds in the open: a
single-sector wave can never reach the top confluence tier on its own, because the
cross-section axis stays provisional until a cross-sector pass confirms sector
leadership as the third independent directional axis. A near-miss the same week
makes the discipline concrete — a name that cleared trend, momentum, and sector
leadership was still refuse-closed when its one-year maximum drawdown crossed the
volatility ceiling by a fraction of a point. A weak veto still blocks. Veto, not a
vote. The full-universe scale-out remains gated on a separate programmatic-credit
lane; the topology is proven and now has citizens in several sectors.

## What this repo is not

- **Not financial advice.** The evaluator checks a portfolio's
  predicate structure against named frameworks; it does not recommend
  trades, position sizes, or strategies. The portfolio agents trade
  paper only.
- **Not a chat surface.** Predicate sub-agents are scoped, audited, and
  short-lived.
- **Not a private-PII surface.** The hosted service accepts only
  redacted portfolios. No real account numbers, real names, or private
  metadata ever flow to the server or to any LLM.

## Cadence

Refreshed weekly from the private working tree. Predicate rewrites,
framework extensions, indicator additions, PM-discipline changes, and
kernel-shape changes are committed as ordinary diffs — the commit log
is the change record. See [`STATUS.md`](./STATUS.md) for the current
release-narrative snapshot.

## Contact

[`quantapix@gmail.com`](mailto:quantapix@gmail.com)

## License

Apache-2.0 (`LICENSE.txt`). Code-class repo — Lean axioms, predicate
stubs, drivers, and the engineering-discipline prose are reused under
the standard Apache patent grant.
