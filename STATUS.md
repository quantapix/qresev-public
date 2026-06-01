# qresev-public — status

_Snapshot: 2026-06-01. Refreshed weekly (Fridays) during the
2026-06-01 → 2026-12-01 drive window._

This is the release-narrative status of the financial-domain slice:
what has landed, where each of the three surfaces stands, and what is
next. It is a companion to the [README](./README.md), not a substitute.

## Overall

**Pre-beta.** The hosted evaluator at `qresev.quantapix.com` enters
early beta on or about **2026-06-01**. The kernel, the predicate specs,
and the supporting market-inspection and portfolio-management surfaces
are live in the private working tree; the public endpoint and its
streaming verification UI are the launch deliverable.

## Formal evaluator (`accounting/`)

- **Five frameworks kernel-green.** Trend, Momentum, OptionsRisk,
  Sector, and Drawdown all elaborate under `lake build`. Top-level
  judgments wired: `is_uptrend` / `is_downtrend`, `has_momentum`,
  `defined_risk_only` / `is_clean`, `has_violation` / `no_violations`,
  `drawdown_disciplined`.
- **Brand-anchor example complete.** One worked portfolio exercises all
  five frameworks across six judgments, with a mix of real-call and
  stubbed predicates, and a full audit trail (per-predicate Bool +
  evidence + uncertainty → generated axioms → kernel verdict).
- **Defined-risk options enforced at the type level** via a closed
  strategy enum, in addition to the authoring-time and runtime gates.
- **Results-propagation pipeline** emits a per-run report, a proof-DAG
  graph, and a status-hub diagram, byte-compatible with the legal-side
  proof-graph rendering kit.

**Next:** per-leg `leg_allowed` enumeration for granular `is_clean`
proofs; parametrised sector caps; porting the sandboxed-driver flags
from the legal-side driver.

## Market inspection (`analyzing/`)

- **Data layer live.** A columnar OHLCV store keyed by symbol, with a
  TA-Lib-parity indicator reference and a GICS sector mapping, refreshed
  across the full S&P 500 universe (~500+ symbols) plus anchor sector
  ETFs.
- **Canonical bar shape** `{ ts, o, h, l, c, v, adj_c }` enforced at the
  ingest and feed boundaries.
- **Coverage status** surfaces through the framework's `/status` board —
  symbol count, indicator-reference count, GICS-mapping presence, and
  newest-bar freshness.

**Next:** the in-editor activity-bar views (Symbols / Sectors /
Portfolios → chart + aggregate panels) and a backtest-summary surface.

## Portfolio management (`trading/`)

- **Three agents on paper.** Aggressive / moderate / conservative, each
  with separate capital, strategy, journal, and broker account.
- **Scheduled routines** (premarket / open / midday / close / overnight)
  run on a cron scheduler; a daily leaderboard reconciles fund return
  vs the benchmark from a single source so all three agents cite
  identical figures.
- **Bull-vs-bear debate gate live** for the `Trend.is_uptrend` judgment
  over the analyzing-side universe — a veto, not a vote, ahead of the
  risk gates.

**Next:** migrate the cron routines to the programmatic SDK lane
(gated on the 2026-06-15 credit activation); the kernel-formalized
**Debate** framework that makes "veto, not a vote" a type-level
property.

## Axiomatize-trading program

Scaffolded — spec, manual-interactive fan-out skill, cell/reconciler
agents, scoring, and the six signal-family axis briefings are drafted.
**Not yet runnable at scale**; Phase-1 gating items (the shared
`Universe/Common` skeleton, the categorization producer, the sandboxed
driver port, the test harness) remain. Scale work is gated on the
2026-06-15 programmatic-credit activation.

## How to verify

- Clone, `lake build`, and watch the kernel either accept or reject the
  worked examples.
- Every indicator predicate's authority resolves to a TA-Lib reference.
- Once the endpoint is live, submit a redacted portfolio and read the
  streamed trace back; nothing un-redacted is accepted.
