# qresev-public — status

_Snapshot: 2026-06-12. Refreshed weekly (Fridays) during the
2026-06-01 → 2026-12-01 drive window._

This is the release-narrative status of the financial-domain slice:
what has landed, where each of the three surfaces stands, and what is
next. It is a companion to the [README](./README.md), not a substitute.

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
- **Brand-anchor example complete** — all five frameworks across six
  judgments, full audit trail, and now replay-faithful on the deployed
  endpoint (see Overall).
- **Per-leg options-book enumeration landed.** The structural cluster
  (per-leg allow-list, naked-short exclusion, debit-only, bounded max
  loss) is proved as shared predicates in the universe library, with
  sorry-free cross-sector equivalence bridges — the granular
  counterpart to the golden kernel's closed strategy enum.
- **Defined-risk options enforced at the type level** via a closed
  strategy enum, in addition to the authoring-time and runtime gates.
- **Results-propagation pipeline** emits a per-run report, a proof-DAG
  graph, and a status-hub diagram; the product's proof surface now
  mounts a recomposed strategy graph with price-chart overlays backed
  by a per-run bars endpoint (landed in the app shell; production
  deploy queued).

**Next:** parametrised sector caps; additional worked examples that
exercise the accept path of the drawdown framework.

## Axiomatize-trading program

- **First top-tier (Tier-A) result.** A targeted-confluence wave
  produced the program's first Tier-A: a real-estate REIT with trend,
  momentum, and cross-section leadership all bullish, every confluence
  bridge sorry-free, and zero bear vetoes. Its sector slice is the
  first per-sector promotion committed into the kernel library as an
  explicit build root; the kernel builds green with the golden
  reference untouched.
- **First accepting Debate run.** `admissible_long` elaborated
  `allowed = true` with no `sorry` for that candidate — and in the same
  wave the strongest-trending name on the board was refuse-closed by
  the volatility veto (~47% historical drawdown). A later wave
  refuse-closed a candidate on a *weak* volume-divergence veto. Veto,
  not a vote, exercised on both branches.
- **Shared predicate library at 34** collapse-bridged predicates
  (from 24 at the last snapshot), including bear-side mirrors,
  an overextension veto, the structural sector map, and the complete
  options-book cluster. The cross-section axis is promoted from
  provisional to confirmed globally.
- **The "limiter is the tape" claim was retracted** — it was a
  pilot-subset artifact of a stale 22-symbol target list. A
  deterministic confluence pre-screen over the full ~526-symbol store
  now front-ends every wave; waves target the survivors. First
  Industrials slice reached Tier-B and is held pending promotion.

Full-universe scale-out stays gated on the programmatic-credit
activation.

## Market inspection (`analyzing/`)

- **Data layer live and current.** The columnar OHLCV store + TA-Lib
  parity indicator reference are refreshed across the full universe
  (526 symbols including anchor sector ETFs, bars current through the
  snapshot week); the pre-screen above runs directly over it.
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
  this week — the agents ran their full scheduled cadence.
- **Scheduled routines** (premarket / open / midday / close /
  overnight) run on a cron scheduler; a daily leaderboard reconciles
  fund return vs the benchmark from a single source.
- **Bull-vs-bear debate gate live** for the `Trend.is_uptrend`
  judgment — and its formalization now has an accepting *and* a
  refusing kernel run on record (see the program section).

**Next:** migrate the cron routines to the programmatic SDK lane
(gated on the credit activation).

## How to verify

- Clone, `lake build`, and watch the kernel either accept or reject the
  worked examples.
- Every indicator predicate's authority resolves to a TA-Lib reference.
- Replay the worked example at the deployed endpoint and read the
  streamed trace back. Freeform portfolio submission opens with the
  beta; nothing un-redacted will be accepted.
