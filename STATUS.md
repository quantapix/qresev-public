# qresev-public — status

_Snapshot: 2026-07-24. Refreshed weekly (Fridays) during the
2026-06-01 → 2026-12-01 drive window._

This is the release-narrative status of the financial-domain slice: what
has landed, where each of the three surfaces stands, and what is next. It
is a companion to the [README](./README.md), not a substitute.

## Correction — 2026-07-17, still open

On 2026-07-14 our own adversarial review lane found leak channels in the
cross-axis **agreement oracle** — the mechanism that scores the top-tier
promotions this program reports. Independent verification cells could, in
principle, observe one another's outputs, so agreement-based tier figures
minted before that date are not established as blind. That correction
stands, and this page does not yet replace the withdrawn figures:

- The **"seven top-tier names"** figure and the "re-tiered up on a fresh
  blind re-slice" claim remain **withdrawn**. The public status board
  reports these tiers as *withheld*, not as a number.
- **Two blind re-slices have since run** under the corrected fan-out
  contracts, on 2026-07-19 and 2026-07-21. Both reproduced a result
  *below* the top tier, both with zero bear vetoes — the cleanest possible
  non-promotion. The count of top-tier names re-earned under the corrected
  contracts is, today, **zero**, and that is what the status board
  publishes. The frozen pre-correction tally is not the live figure and is
  not treated as one.
- **Unaffected: kernel soundness and the encoded census.** Every
  `lake build` is green and every committed encoding is `sorry`-free. How
  many symbols are encoded is a mechanical count, not an agreement figure.
  The accepting-and-refusing Debate behaviour and the defined-risk options
  refusal are kernel facts — they stand.
- The corrected tier figures publish in a single dated update carrying all
  axes together, not ahead of it, so that no page vouches for another
  mid-correction. Earlier weekly digests are dated records, left intact;
  this correction supersedes their tier language.

We surfaced this because the cells check one another and one lane caught
it — not because the earlier numbers were right all along.

## Overall

**Pre-beta — endpoint deployed, beta not yet declared.** The hosted
evaluator at `qresev.quantapix.com` is deployed and reachable: the app
shell and its API answer publicly, and the worked example streams a kernel
verdict back that matches the committed ground truth. The drive's
early-beta opening — the hosted verification-service deliverable — remains
scheduled for Month 3+ of the window. Until it is declared, the deployment
is an engineering surface, not the launch deliverable.

## Formal evaluator (`accounting/`)

- **Eight frameworks kernel-green.** The five golden frameworks (Trend,
  Momentum, OptionsRisk, Sector, Drawdown) are joined by Regime,
  ExitDiscipline, and Hedging under a chartered expansion. All elaborate
  under `lake build`; the spec battery is 47/47 green.
- **Hedging judgments landed.** Both top-level Hedging judgments now
  elaborate as whole structures with no omittable field — a
  sizing-and-volatility-and-floor conjunction, and a consistency judgment
  tying a hedge's presence to the prevailing regime. Eight of eleven
  predicates are in; the remaining three are **bound on a portfolio-schema
  extension**, not on kernel work. They are recorded as blocked rather than
  stubbed — a predicate that cannot yet read its evidence does not get a
  placeholder.
- **Depth, not just breadth.** A momentum percentile predicate was
  decomposed into a rank-versus-cohort shape, continuing the pattern where
  a model-backed judgment is pushed down to independently-decidable leaves
  and the composite is derived in the kernel.
- **The evidence tape is now pinned into every run.** Three gates were
  added between the kernel program and its market-data ground truth:
  - a **corpus pin** — each archived run records the content hash of the
    exact tape it read, so "which data was this proved against" is a
    lookup, not a reconstruction;
  - a **trust gate** — a run refuses to start if a symbol in its scope
    carries an unmodelled data restatement, an unreconciled refusal, or a
    frozen series. It is scoped to the run's own symbols: an
    everything-scoped version would abort every run forever on one
    permanently-blocked symbol, which is how a gate becomes noise;
  - a **coherence gate** — if the tape's content hash changed *during* a
    run, the run's cells measured different worlds and its agreement is
    void. The remedy is to re-run against a pinned tape, **never** to
    re-score the agreement. An absent witness fails closed.

  The coherence check is deliberately not a self-report. Asking each cell
  which tape it read makes the check exactly as reliable as the cell's
  compliance; instead the single data-access boundary stamps every read,
  and the archiver asks whether that set of stamps is split. A cell cannot
  forget to be honest about something it never touches.
- **Blindness is now certified per run, not asserted.** Each archived run
  carries a transcript audit over all four known leak channels (shared
  scratch space, orchestrator prompting, sibling reads, and recalled prior
  adjudications). The two re-slices in this window each returned
  ENFORCED-BLIND-CLEAN for every blind cell — the certification the
  pre-correction runs could never be given.
- **A guard that cannot be observed failing is not a guard.** Two
  self-inflicted findings this window are worth publishing because they
  generalise. A new guard's test passed green against a guard that never
  ran — the assertion matched text the tool emits on *any* misuse, so it
  proved nothing; the rule now is to assert on a string only the code under
  test can emit, and to drive the tool with a complete invocation. And a
  new data census was accepted only after three fault classes were injected
  into it and all three were caught. A census that cannot fail is worse
  than no census, because it is believed.
- **Refusing to run is a reported outcome.** The deterministic pre-filter
  that targets a run — pure indicator and relative-strength arithmetic, no
  model calls — returned zero eligible candidates on the most recent tapes,
  so no run was launched and no model spend was incurred. Those refusals
  are now recorded as first-class rows in the program's history rather than
  living in prose, so the axis does not read as idle when it is in fact
  correctly holding.

## Axiomatize-trading program

- **Encoded census: 10 symbols across four sectors**, each committed into
  the kernel library as an explicit build root, each `sorry`-free, with the
  golden reference untouched. Counting only committed universe encodings
  (the golden calibration portfolios are scaffolding, not universe
  members), that is on the order of **1.9%** of the ~526-symbol GICS
  universe. This is a mechanical census and is unaffected by the agreement
  correction above.
- **Tier counts: withheld.** See the correction. Where tiers are reported
  at all, they are derived from per-symbol enumeration and never carried as
  a standalone number.
- **Two blind re-slices, both non-promoting.** Each ran the full pipeline —
  seven independent signal-family cells, a per-sector reconciliation, a
  cross-sector pass, and the Debate gate — and each landed below the top
  tier because only two independent directional axes were confirmed at
  single-sector scope; the cross-section axis stays provisional until a
  cross-sector pass. Both had **zero bear vetoes**. In both, the Debate
  gate correctly **refuse-closed** with no `sorry`: the admissibility
  structure is unconstructable without the top-tier confluence claim. One
  of the two names is the program's original *accepting* exemplar, and it
  now correctly refuses. That is the anti-echo property working — a
  re-verification that could only ever confirm the prior answer would be
  measuring nothing.
- **The oracle caught a real defect in-flight.** In one re-slice, a
  reconciliation build-guard caught a corroborator cell reporting success
  on a claim that had discharged *vacuously*. The cell was
  tier-neutral, so the verdict did not move — but the guard, not a human
  review, is what found it.
- **Sharing across axes is measured, not assumed.** A shared predicate
  library backs the per-sector slices; each new run reports how many of its
  candidate predicates re-witness existing shared cores versus mint new
  ones, so consolidation is observable rather than claimed.

Full-universe scale-out remains gated on a separate programmatic lane.

## Market inspection (`analyzing/`)

- **The local inspection surface is feature-complete for its first
  workflow set.** Its phases all shipped this window: a cross-section
  screen over the frontier bar, a pairwise correlation matrix and rolling
  beta view, a per-book conditions board, and side-by-side compare grids.
  It runs on loopback only, is absent from every deploy lane, and renders
  **ground-truth tape only** — no predicate verdict, framework judgment, or
  proof node appears in an inspection panel. Anything exported off it
  becomes a financial-clearance subject.
- **Honesty rules are mechanical, not stylistic.** A correlation pair with
  fewer overlapping returns than the window requires is reported as
  undecidable rather than as a short-window number. Symbols with too few
  bars for a cohort are listed as excluded rather than silently composited.
  A book's beta reference lines render as **values only** — there is no
  in-band / out-of-band verdict word, because the kernel predicate that
  would license one does not exist yet.
- **Restatement classification is compositional.** A vendor refresh that
  changes historical values is decomposed into world-fact components —
  split-like, dividend-like, volume-settlement, and residual — instead of a
  single opinion word. The latest full refresh came back with **zero
  unmodelled residuals**, which is what licenses the claim that no new
  supplier-change class needs naming. A refinement this window: rescale
  uniformity is judged over a column's *changed* bars only. A corporate
  action rescales bars before its effective date, and mixing the unchanged
  tail into the ratio set had been mis-classing routine dividends as
  irregular.
- **History and membership are both monotone.** A refresh may add bars and
  may restate them, but may never reduce the set of dates held for a
  symbol; a vendor drop is refused, the file left byte-identical, and the
  run exits non-zero rather than letting a downstream program rebase on a
  truncated series. One symbol has been sitting in that refused state for
  weeks — held on the record rather than repaired, now for the sixth
  consecutive refresh. Universe membership is monotone too: a constituent
  removal is a **retire-in-place**, and the freshness manifest's
  drop-detection was widened this window by exactly the
  retired-and-still-held set — a per-symbol intersection, never a
  tolerance. A retirement and a genuine silent drop in the same refresh do
  not cancel each other out.
- **Coverage status** surfaces through the framework's `/status` board —
  symbol count, indicator-reference count, GICS-mapping presence, and
  newest-bar freshness.

**Next:** a backtest-summary surface, and per-sector breadth once a
materialised source for it exists (today it is unshipped by design rather
than approximated).

## Portfolio management (`trading/`)

- **Three agents on paper.** Aggressive / moderate / conservative, each
  with separate capital, strategy, journal, and broker account. The
  discipline rules — paper-only, defined-risk-only, not-day-trading, risk
  gate before execution, separate capital, journal-before-quitting — are
  unchanged this window.
- **Refuse rather than fabricate.** The options-math helper now raises at
  or past expiry instead of returning zero-filled Greeks, and failure text
  goes to standard error with standard output left empty, so a caller
  parsing output can never mistake an error message for a result.
- **Scheduled routines are dependency chains, not independent fires.** A
  day's routines hand state to each other through tracked files in the
  repository, so a chain must run start-to-finish on one machine. That was
  never modelled, and a mid-morning handover between workstations split a
  day across two of them: every gate returned the correct verdict, and the
  day's records still ended up contradictory, with one pre-committed ticket
  never placed. The fix is a third invariant alongside exclusivity and
  capability — **continuity**: a tenure may only begin inside a fixed
  evening changeover hour, when no chain is in flight, and the incoming
  machine must fast-forward to the authoritative branch or refuse outright.
  A missed run is recoverable; a double run corrupts the book. We take the
  gap and make it loud.
- **Probes must import, not ask the packager.** A pre-flight check returned
  green on twenty-four of twenty-four capabilities on a machine that could
  not compute a single option Greek: a numerics library's metadata
  directory was present while its package directory was gone, so the
  package manager reported it installed and a reinstall was a silent no-op.
  A capability map that asks the installer what is installed measures the
  installer.
- **A process cannot diagnose its own run.** The scheduling wrapper writes
  a run's exit code and cost *after* the run returns, so a routine
  inspecting its own log always sees it truncated and can only conclude it
  crashed. One did, reported a phantom failure, and the false report
  propagated into a committed record before it was caught. The absence of a
  trailing line is structural, not evidence.

**Next:** finish the mechanical write-fence and session-calendar migration
for the remaining routine producers.

## How to verify

- Clone, `lake build`, and watch the kernel either accept or reject the
  worked examples.
- Every indicator predicate's authority resolves to a TA-Lib reference.
- Replay the worked example at the deployed endpoint and read the streamed
  trace back. Freeform portfolio submission opens with the beta; nothing
  un-redacted will be accepted.
- Where this page reports a tier, check it against the status board first —
  during the correction the board's *withheld* is the authority, and no
  number on this page outranks it.
