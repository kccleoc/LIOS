# 02 · Capital Architecture — Five Sleeves A/B/C/D/Z

**Component version:** v4.1
**Status:** Constitutional (P1, P2, P6)
**Last updated:** 2026-11-14

---

## Purpose

All capital sits in exactly five sleeves, measured against **one denominator:
NLV**.

```
NLV = A + B + C + D + Z
```

| Sleeve | Name | Purpose | Risk role |
|---|---|---|---|
| **A** | Core Index | Permanent compounding core | Systematic beta |
| **B** | Conviction | 1–4 individual equities, equal weight at entry | Idiosyncratic alpha |
| **C** | Satellite | Discretionary rotation, winner-weighted | Active thematic |
| **D** | Duration | Bonds >3y, currency-hedged | Ballast + ammunition |
| **Z** | Cash Equivalents | Duration ≈ 0 | Liquidity |

A + B + C = risky block. D + Z = defensive block.

---

## Sleeve A — Core Index

Broad-index ETFs only, at fixed within-sleeve weights summing to 100%.
No individual equities, no thematic funds, no bonds, no cash-like instruments.
The core is never sold to fund another sleeve.

A cash-like ETF inside A is prohibited in v3.0: cash belongs in Z. The sleeve is
defined by risk factor, not by ticker or issuer.

---

## Sleeve B — Conviction

1–4 names, equal weight at entry. Slot bands:

| Names | Slot | Drift band |
|---|---:|---|
| 1 | 100% | 60–100% |
| 2 | 50% | 35–65% |
| 3 | 33.3% | 20–50% |
| 4 | 25% | 15–35% |

**The only sell trigger is a thesis break** — LOS < 60 or equivalent research
failure. Price is never a trigger. Caps apply to **effective** exposure (direct
plus index-embedded), not to the direct holding alone.

---

## Sleeve C — Satellite

Winner-weighted by design. No equal-weight rule. Laggards are cut, never averaged
down. Funded only from inside C. Quarterly ranking against the sleeve benchmark;
bottom half for two consecutive quarters requires a cut or a written
justification. Above the ceiling, trim the **weakest** name.

All Sleeve C surgery is governed by the Replacement Index.

---

## Sleeve D — Duration

Duration greater than 3 years, hedged to the liability currency. T-bills,
money-market funds and 0–1y instruments are **not** D; they are Z.

D has two jobs: ballast, and ammunition. It is the designated source for the
rebalance rule and for the discretionary channel. It never funds B or C.

D hedges growth and deflation shocks. It does not hedge inflation shocks — in
2022 duration fell alongside equities. That is accepted.

---

## Sleeve Z — Cash Equivalents

Duration ≈ 0. Defined by risk factor, not issuer. Never reported as a bond
sleeve or a hedge. The emergency fund sits **outside** NLV.

Above ceiling → redeploy to the most underweight sleeve.
Below floor → funded first from incoming contributions.

---

## Why the denominator changed

Pre-v3.0 measured Sleeve A and Sleeve C against `ABC = NLV − D − SR` while
measuring Sleeve B caps against NLV. One dollar carried two weights. The
resulting gap widened when it was funded, because new cash entered the
denominator as well as the numerator, and no deployment rule could close it.

v3.0 deletes ABC. Drift and gap are the same number.

---

## Constraint Set

| # | Constraint | Parameter |
|---|---|---|
| 1 | Single-name effective exposure | `sleeves.b.position_cap_pct` |
| 2 | Single-factor exposure | `constraints.factor_cap_pct` |
| 3 | Duration hedge | `sleeves.d.target_pct` |
| 4 | Total risky weight | `glidepath.bands` |
| 5 | US-situs assets | `constraints.us_situs_ceiling` |
| 6 | Satellite ceiling | `sleeves.c.ceiling_pct` |

Targets are outputs of constraints. A target that violates a constraint is
invalid.

---

## Removed in v3.0

| Concept | Fate |
|---|---|
| `ABC` denominator | Deleted |
| Strategic Reserve | Repealed (L6) |
| Kelly Reserve | Deleted |
| Free tactical bucket | Deleted |
| Cash-like ETF inside A | Prohibited |

---

See also: `core/sleeves.md`, `core/deployment.md`, `core/regime-index.md`,
`core/glidepath.md`.
