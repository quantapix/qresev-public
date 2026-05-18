# qresev-public

> Lean4 axiomatic theorem-proving with LLM-backed predicate functions
> for the **financial** domain. The redacted public slice of the
> `accounting/` subproject. Backs the **Qresev** product.

A weekly-refreshed window into the formal-financial kernel that runs
alongside the private working repository. Same three-layer split as
[`qnarre-public`](https://github.com/quantapix/qnarre-public),
different domain: instead of complaints under federal statutes,
this kernel proves portfolio-level judgments over OHLCV bars +
indicator series + GICS sector mappings.

- Parent organisation: <https://github.com/quantapix>
- Engineering output: <https://quantapix.com>
- Live product: <https://qresev.quantapix.com>

## The three-layer split

| Layer | Reads | Writes | Tools |
|---|---|---|---|
| **Formal kernel** (`Accounting/<Framework>/*.lean`) | only Lean | only Lean | Lean elaborator. No I/O, no LLM calls. |
| **Predicate functions** (`predicates/<framework>/*.md`) | one portfolio JSON + bar/indicator evidence | a single `Bool` (plus evidence + uncertainty) | LLM sub-agent with `context: fork`. |
| **Driver** (`scripts/extract_facts.py`) | manifest + portfolio | `Accounting/<Framework>/Facts.lean` (axioms) + audit JSON | LLM `--print --model haiku` invocations. |

The Lean kernel never reads OHLCV bars, indicator series, parquet, or
JSON. The predicate sub-agents never write Lean. The driver is a
thin coordinator with no financial reasoning of its own. The
verifiable proof IS the Lean elaboration trace produced by
`lake build`.

## Frameworks

Five frameworks, each under `Accounting/<Framework>/` (kernel) +
`predicates/<framework>/` (specs).

| Framework | Kernel | Top-level judgment | Predicate count |
|---|---|---|---:|
| **Trend** | `Accounting/Trend/` | `is_uptrend`, `is_downtrend` | 5 |
| **Momentum** | `Accounting/Momentum/` | `has_momentum` | 4 |
| **OptionsRisk** | `Accounting/OptionsRisk/` | `defined_risk_only`, `is_clean` | 6 |
| **Sector** | `Accounting/Sector/` | `has_violation`, `no_violations` | 3 |
| **Drawdown** | `Accounting/Drawdown/` | `drawdown_disciplined` | 3 |

The kernel's predicate names are the contract the Qresev `/app`
Lean trace consumes. They do not get renamed without lockstep UI
edits.

## Defined-risk options — hard refusal at the boundary

The `OptionsRisk` framework enforces a six-strategy allow-list:

- `long_call`, `long_put`
- `debit_spread_call`, `debit_spread_put`
- `covered_call`, `protective_put`

Anything outside that set — naked shorts, undefined-risk multi-leg
combos, leveraged structures — is refused at two surfaces:

1. **UI** — Qresev hard-refuses to even render a portfolio that
   includes a disallowed leg. The user sees a refusal card with the
   specific leg cited; no kernel call fires.
2. **Kernel** — `OptionsRisk.is_clean` requires `defined_risk_only` as
   a precondition; the theorem cannot elaborate against a portfolio
   that violates the allow-list.

This is a load-bearing project rule, not a tunable. The same
six-strategy gate is enforced at authoring time across the analyzer
side as well.

## Canonical OHLCV bar shape

Every bar that enters the predicate evidence carries the canonical
column set: `{ ts, o, h, l, c, v, adj_c }`. Vendor field names die
at the boundary. The kernel never sees vendor strings.

## GICS sector mapping

The `Sector` framework reads the canonical GICS hierarchy from a
shared Parquet file keyed by symbol. Sector strings are the eleven
canonical GICS long-form names; abbreviations and shorthand do not
parse. A sector-cap claim like "max 25% Information Technology" is a
Lean `Prop` with a structured witness, not a spreadsheet count.

## Predicate spec shape

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
JSON object with `{ value: Bool, evidence: [...], uncertainty:
[0,1) }`.

## TA-Lib parity

Indicator predicates that reference `RSI`, `MACD`, `ADX`, `SMA` etc.
match TA-Lib semantics exactly — including the MACD EMA-realignment
quirk where TA-Lib aligns both EMAs to the slow-period start. The
parity check runs on every framework that touches indicators.

## Build and verify

```
lake build
```

Either the kernel elaborates against the generated facts — the
portfolio judgment holds across all five frameworks — or it does
not, in which case the failing theorem names the predicate that
does not provide enough evidence under the current axiom set.

## Worked examples

`examples/<id>/` holds per-portfolio artifacts: the redacted
portfolio JSON, the bar evidence view, `facts.json`, the generated
axiom block, the driver's audit log, and the Lean elaboration trace.

## What this repo is not

- Not financial advice. The evaluator checks a portfolio's
  predicate structure against named frameworks; it does not
  recommend trades, position sizes, or strategies.
- Not a chat surface. Predicate sub-agents are scoped, audited, and
  short-lived.
- Not a private-PII surface. The hosted service accepts only
  redacted portfolios. No real account numbers, real names, or
  private metadata ever flow to the server or to any LLM.

## Cadence

Refreshed weekly from the private working tree. Predicate rewrites,
framework extensions, indicator additions, and kernel-shape changes
are committed as ordinary diffs — the commit log is the change
record.

## Contact

[`quantapix@gmail.com`](mailto:quantapix@gmail.com)

## License

MIT.
