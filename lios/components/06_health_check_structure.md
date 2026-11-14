# 06 · Health Check Structure (16 Sections)

**Component version:** v3.0
**Status:** Locked Decision (L10 — structure)
**Last updated:** 2026-11-14

---

## Purpose

The Health Check is the canonical output format for a full session review.
**Always 16 sections, always this order.** Deviating breaks cross-session
comparability.

The authoritative specification is `core/health-check.md`. This file is a
reference summary.

---

## The 16 Sections

| # | Section | Output |
|---|---|---|
| 1 | Portfolio Health | NLV, sleeve totals vs targets, reconciliation |
| 2 | Gap & Drift | One table — gap and drift are the same number |
| 3 | Bull–Bear Index | Five scores, weighted BBI, band, band change |
| 4 | Constraint Check | Single name, factor, duration, risky, US-situs |
| 5 | Sleeve A Review | Holdings, within-A weight drift |
| 6 | Sleeve B Review | LOS, thesis-exit status, slot drift, verdicts |
| 7 | Sleeve C Review | Rotation ranking, ceiling, laggard cut list |
| 8 | Sleeve D Review | Duration, currency hedge, ammunition |
| 9 | Sleeve Z Review | Floor and ceiling |
| 10 | Deployment Plan | Rule 1 output in dollars |
| 11 | Discretionary Channel | Open/closed, remaining budget |
| 12 | Frontier Scoreboard | C names + candidates |
| 13 | Execution Calendar | Earnings, macro, index events |
| 14 | Execution Plan | Buy/Hold/Reduce/Exit with dollars |
| 15 | Risk Assessment | Constraints + drawdown ladder in dollars |
| 16 | CIO Resolution | One paragraph, confidence % |

---

## Section 3 — Bull–Bear Index

```
BBI = 0.30·MV + 0.25·PV + 0.20·ML + 0.15·T + 0.10·O
```

0–100, higher = more bearish. Bands: Risk-On 0–35 · Constructive 36–50 ·
Cautious 51–65 · Defensive 66–100.

The index shifts the A/B/C split inside a fixed risky budget. **It never changes
total risky exposure** (L21).

## Section 4 — Constraint Check

Always compute **effective** exposure: direct weight plus index-embedded weight.
A direct position inside the cap can still breach on an effective basis.

## Section 10 — Deployment Plan

Show `NLV'`, the gap per sleeve, `ΣG⁺`, and the allocation. If Rule 2 is
triggered, state that it absorbs the contribution and Rule 1 is not run
separately.

## Section 15 — Risk Assessment

Restate the drawdown ladder **in dollars**: ordinary bad quarter, bad quarter,
bad year, severe drawdown. If any number is unacceptable, the fix is the
glidepath — not a market call.

---

## Writing Rules

- Conclusions first, reasoning second
- Dollar amounts always, never percentages alone
- Facts separated from assessments
- Confidence % on sections 5, 6, 8, 14, 15
- No skipped sections — "no action" plus a one-line rationale is acceptable
- No Kelly Factor, no Strategic Reserve, no `ABC` denominator

---

See also: `core/health-check.md`, `core/deployment.md`, `core/regime-index.md`.
