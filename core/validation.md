# LIOS v3.0 Auto-Validation Rules

**Version:** 3.0
**Last updated:** 2026-11-14

Every report generation runs these **9 checks** against
`portfolio/state/portfolio.yaml`. If any check fails, generation is aborted.

## Validation Pipeline

All checks execute in order. Each reads from `portfolio/state/portfolio.yaml`.
A failure halts the pipeline and reports the failure with details.

---

### Check 1 · Position Count

**Verifies:** the number of positions matches the last known valid count.

**Source:** `positions` array length

**Failure:** count differs from last valid count, or is zero.

**Abort:** "Validation failed: unexpected position count (got X, expected Y)."

---

### Check 2 · Share Count

**Verifies:** every position has a positive, numeric share count.

**Source:** `positions[].shares`

**Failure:** any `shares ≤ 0`, or not a number.

**Abort:** "Validation failed: invalid share count for [TICKER]."

---

### Check 3 · Average Cost

**Verifies:** every position has a positive, numeric average cost.

**Source:** `positions[].avg_cost`

**Failure:** any `avg_cost ≤ 0`, or not a number.

**Abort:** "Validation failed: invalid average cost for [TICKER]."

---

### Check 4 · Sleeve Assignment

**Verifies:** every position is assigned to a valid v3 sleeve.

**Source:** `positions[].sleeve`

**Failure:** any sleeve value not in `{a, b, c, d, z}`.

**Abort:** "Validation failed: invalid sleeve assignment for [TICKER]."

**Note:** the v3 sleeve set is **A, B, C, D, Z**. `sleeve: sr` is invalid —
the Strategic Reserve was repealed in v3.0.

---

### Check 5 · Sleeve Targets Sum to 100

**Verifies:** target percentages are internally consistent and match the age
band in `glidepath`.

**Source:** `sleeves.a.target_pct` … `sleeves.z.target_pct`, `glidepath.bands`

**Failure:**
- targets do not sum to 100 (±0.1)
- the target set does not equal the glidepath band for the investor's age
- `A + B + C` falls outside `constraints.risky_min_pct` … `risky_max_pct`

**Abort:** "Validation failed: sleeve targets sum to X, expected 100."

---

### Check 6 · Sleeve Risk-Factor Purity

**Verifies:** each sleeve holds instruments consistent with its definition.

**Source:** `positions[]` joined with instrument metadata

**Failure:**
- a cash-like instrument (duration < 1y) assigned to sleeve `a` or `d`
- a duration-bearing instrument (duration > 3y) assigned to sleeve `z`
- an individual equity assigned to sleeve `a`
- a broad-index ETF assigned to sleeve `b`

**Abort:** "Validation failed: [TICKER] violates the risk-factor definition of
sleeve [X]."

**Note:** this is the check that prevents the v2.x error of booking a 0–1y
Treasury fund as a bond sleeve. Duration is the test, not the issuer.

---

### Check 7 · Bull–Bear Index Validity

**Verifies:** the index is computed, complete and consistent with its band.

**Source:** `bbi.*`

**Failure:**
- any dimension missing, or outside 0–100
- `weighted` does not equal `0.30·MV + 0.25·PV + 0.20·ML + 0.15·T + 0.10·O`
  (within 0.1)
- `band` does not match the weighted value
  (0–35 Risk-On / 36–50 Constructive / 51–65 Cautious / 66–100 Defensive)
- more than one band step versus the prior cycle

**Abort:** "Validation failed: BBI inconsistent (weighted X, band Y)."

**Note:** a second band step in the same quarter is a rule violation (L21), not
a data error. Report it as a compliance failure, not an abort if the data is
otherwise sound.

---

### Check 8 · Market Value

**Verifies:** every position has a valid, positive market value.

**Source:** `positions[].market_value`

**Failure:** any `market_value ≤ 0`, or not a number.

**Abort:** "Validation failed: invalid market value for [TICKER]."

---

### Check 9 · NLV Reconciliation

**Verifies:** authoritative NLV equals the sum of position market values plus
cash.

**Source:** `nlv`, `portfolio_total`, optional `ibkr_nlv`

**NLV policy (v3.0):**
- `nlv` must equal `Σ(positions[].market_value) + cash.balance` — computed
- `ibkr_nlv`, if present, is broker-reported and is **reference only**
- every LIOS calculation uses `nlv`, never `ibkr_nlv`
- there is one denominator. `ABC` does not exist in v3.0

**Failure:** `|nlv − portfolio_total| > $1.00`. No exceptions, no soft pass.

**Non-failure (INFO only):** `|ibkr_nlv − nlv|` may exceed $1.00. Log the gap, do
not abort.

**Abort:** "RECONCILIATION FAILED. Delta: $X.XX. Do not generate reports until
state is corrected and all 9 checks pass."

---

## Validation Log

Every run records, in `logs/validation/`:

- timestamp
- snapshot reference
- NLV and portfolio total
- reconciliation delta
- validation status (PASSED / FAILED)
- per-check results with details

## Agent Instructions

Before generating any report:

1. Load `portfolio/state/portfolio.yaml`
2. Run all 9 checks in order
3. If any check fails → abort, record in `logs/validation/`, report the failure
4. If all pass → record, proceed

Never generate a report from an unvalidated or failed state.

---

## Removed in v3.0

| v2.x check | Fate |
|---|---|
| Strategic Reserve non-negative | Deleted — no SR exists |
| Kelly Reserve non-negative | Deleted — no KR exists |
| Cash sub-bucket reconciliation (SR/KR/TC/DCA) | Deleted — one cash balance, one Z sleeve |

Replaced by Check 5 (target consistency) and Check 6 (risk-factor purity), which
catch the class of error the old cash checks could not see.
