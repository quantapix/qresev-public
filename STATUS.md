# qresev-public — status

_Snapshot: 2026-07-17. Refreshed weekly (Fridays) during the
2026-06-01 → 2026-12-01 drive window._

This is the release-narrative status of the financial-domain slice:
what has landed, where each of the three surfaces stands, and what is
next. It is a companion to the [README](./README.md), not a substitute.

## Correction — 2026-07-17

On 2026-07-14 our own adversarial review lane found leak channels in the
cross-axis **agreement oracle** — the mechanism that scored the Tier-A /
Tier-B promotions described below. Independent verification cells could,
in principle, observe one another's outputs, so the agreement-based tier
figures minted before that date are not established as blind.
Accordingly:

- The **"seven Tier-A names"** figure and the "re-tiered up on a fresh
  blind re-slice" claim are **withdrawn** pending re-verification. A
  blind re-slice of the still-eligible names has since been completed
  under the corrected contracts; the corrected figure publishes with the
  single dated status update that carries all axes together, not ahead of
  it — so no page vouches for another mid-correction.
- What is unaffected: **kernel soundness.** Every `lake build` is green
  and every promoted slice is `sorry`-free; the accepting/refusing
  `admissible_long` Debate behavior and the defined-risk options refusal
  are mechanical kernel facts, not agreement figures — they stand.
- The financial sign-off posture treats any surface derived from the
  agreement tiers as suspended until the re-slice lands. Earlier weekly
  digests are dated records, left intact; this correction supersedes
  their tier language.

We surfaced this because the cells check one another and one lane caught
it — not because the earlier numbers were right all along.

## Overall

**Pre-beta — endpoint deployed, beta not yet declared.** The hosted
evaluator at `qresev.quantapix.com` is deployed and reachable: the app
shell and its API answer publicly, and the worked example streams a
kernel verdict back that matches the committed ground truth. A
replay-faithfulness bug found via the deployed endpoint (stub predicate
values were spec-keyed and polarity-blind, flipping a drawdown verdict)
was fixed at the source — stub values now seed from the example's
committed facts — and re-verified against the deployed service. The
drive's early-beta opening (the hosted verification-service deliverable)
remains scheduled for Month 3+ of the window; until it is declared, the
deployment is an engineering surface, not the launch deliverable.

## Formal evaluator (`accounting/`)

- **Five frameworks kernel-green.** Trend, Momentum, OptionsRisk,
  Sector, and Drawdown all elaborate under `lake build`. Top-level
  judgments wired: `is_uptrend` / `is_downtrend`, `has_momentum`,
  `defined_risk_only` / `is_clean`, `has_violation` / `no_violations`,
  `drawdown_disciplined`.
- **Hierarchical decomposition landed.** Three frameworks now decompose
  their top-level judgment into independently-decidable leaves with a
  **kernel-derived** composite: the momentum MACD cross (a disjunction
  over its leaves), the trend SMA cross (five moving-average leaves over
  two routes), and the maximum-drawdown veto (a single measured
  magnitude bounded against its ceiling). Numeric comparisons lift to
  integer arithmetic the elaborator settles directly (`decide` / `omega`,
  no model call in the loop). The flat golden judgments are byte-for-byte
  unchanged — decomposition is an additive, more granular proof of the
  same conclusion.
- **Drawdown accept path now exercised.** A conservative cash-heavy
  example portfolio elaborates `drawdown_disciplined` alongside a clean
  options book and a sector-cap proof under a tightened house policy —
  closing the prior "Next" item.
- **Per-leg options audit, v2.** The structural options-book audit was
  folded into a single stronger predicate (per-leg structural cleanliness)
  and proved to *imply* the prior v1 audit. The user-facing `is_clean` /
  `defined_risk_only` judgments are byte-stable.
- **3-PM golden triad closed.** The conservative-PM example completed the
  three-agent golden set — a cash-heavy book with three ETFs clears
  sector concentration under a tightened policy, drawdown discipline, and
  a clean options book, all real-call.
- **Parametrised sector caps shipped** — the concentration cap is a cap
  policy (GICS sector → ceiling), with the default definitionally equal
  to the old fixed ceiling, so the public judgment surface is unchanged.

## Axiomatize-trading program

- **Seven top-tier (Tier-A) names across three sectors.** Promotions now
  span industrials, real estate, and utilities — each reconciled
  sorry-free, each committed into the kernel library as an explicit build
  root, with the golden reference untouched. The utilities slice is the
  third promoted sector. A name re-tiered up this week after both of its
  prior vetoes cleared on a fresh blind re-slice.
- **Accepting and refusing Debate runs on record.** `admissible_long`
  elaborates `allowed = true` with no `sorry` for cleared candidates; a
  near-miss the same week was refuse-closed when its one-year maximum
  drawdown crossed the volatility ceiling by a fraction of a point. A
  weak veto still blocks. Veto, not a vote.
- **Coverage published truthfully on `/status`.** Counting only committed
  Universe-Bridge encodings (the golden calibration portfolio is excluded
  by design), the encoded share of the ~526-symbol universe is on the
  order of 1.7%, every encoding sorry-free. Tier counts are derived from
  per-symbol enumeration, never carried as a standalone number.
- **Shared predicate library at 34** collapse-bridged predicates,
  including bear-side mirrors, an overextension veto, the structural
  sector map, and the complete options-book cluster.

Full-universe scale-out stays gated on the programmatic-credit
activation.

## Market inspection (`analyzing/`)

- **Data layer live and current.** The columnar OHLCV store + TA-Lib
  parity indicator reference are refreshed across the full universe
  (526 symbols including anchor sector ETFs, bars current through the
  snapshot week); the deterministic three-stage targeting filter runs
  directly over it. No indicator-surface or ingest-shape change this
  window.
- **Canonical bar shape** `{ ts, o, h, l, c, v, adj_c }` enforced at
  the ingest and feed boundaries.
- **Coverage status** surfaces through the framework's `/status`
  board — symbol count, indicator-reference count, GICS-mapping
  presence, and newest-bar freshness.

**Next:** the in-editor activity-bar views (Symbols / Sectors /
Portfolios → chart + aggregate panels) and a backtest-summary surface.

## Portfolio management (`trading/`)

- **Three agents on paper.** Aggressive / moderate / conservative, each
  with separate capital, strategy, journal, and broker account. The
  discipline rules (paper-only, defined-risk-only, not-day-trading,
  risk gate, separate capital, journal-before-quitting) are unchanged
  this window — the agents ran their full scheduled cadence.
- **Scheduled routines** (premarket / open / midday / close /
  overnight) run on a cron scheduler; a daily leaderboard reconciles
  fund return vs the benchmark from a single source.
- **Bull-vs-bear debate gate live** for the `Trend.is_uptrend`
  judgment over the ~526-symbol universe — its formalization now has an
  accepting *and* a refusing kernel run on record (see the program
  section). Unchanged this window.

**Next:** migrate the cron routines to the programmatic SDK lane
(gated on the credit activation).

## How to verify

- Clone, `lake build`, and watch the kernel either accept or reject the
  worked examples.
- Every indicator predicate's authority resolves to a TA-Lib reference.
- Replay the worked example at the deployed endpoint and read the
  streamed trace back. Freeform portfolio submission opens with the
  beta; nothing un-redacted will be accepted.
