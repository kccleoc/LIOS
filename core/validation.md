> **v3.0 conformance note.** Written for LIOS v2.x. In v3.0 the sleeves are
> **A/B/C/D/Z** measured against NLV, the Kelly Factor is replaced by the
> **Bull–Bear Index**, and the Strategic Reserve is repealed. Where this file
> and `lios/v3.0.md` disagree, v3.0 wins.


# LIOS Auto-Validation Rules
# Version: 2.1.0
#
# ⚠️  Every report generation runs these 9 checks against portfolio/state/portfolio.yaml.
# If ANY check fails, generation is ABORTED.

## Validation Pipeline

All checks execute in order. Each check reads from `portfolio/state/portfolio.yaml`.
Failure at any check halts the pipeline and reports the failure with details.

---

### Check 1: Position Count Validation

**What it verifies:** The number of positions matches the expected count.

**Source:** `portfolio/state/portfolio.yaml` → `positions` array length

**Failure condition:** Position count differs from last known valid count (or is zero).

**Abort behavior:** Stop generation. Report: "Validation failed: unexpected position count (got X, expected Y)."

---

### Check 2: Share Count Validation

**What it verifies:** Every position has a positive, non-zero share count.

**Source:** `portfolio/state/portfolio.yaml` → `positions[].shares`

**Failure condition:** Any position has shares ≤ 0 or shares is not a number.

**Abort behavior:** Stop generation. Report: "Validation failed: invalid share count for [TICKER]."

---

### Check 3: Average Cost Validation

**What it verifies:** Every position has a valid average cost.

**Source:** `portfolio/state/portfolio.yaml` → `positions[].avg_cost`

**Failure condition:** Any position has avg_cost ≤ 0 or avg_cost is not a number.

**Abort behavior:** Stop generation. Report: "Validation failed: invalid average cost for [TICKER]."

---

### Check 4: Sleeve Validation

**What it verifies:** Every position is assigned to a valid sleeve.

**Source:** `portfolio/state/portfolio.yaml` → `positions[].sleeve`

**Failure condition:** Any position has a sleeve value not in {A, B, C, D}.

**Abort behavior:** Stop generation. Report: "Validation failed: invalid sleeve assignment for [TICKER]."

---

### Check 5: Cash Validation

**What it verifies:** Cash balance is consistent with sub-buckets.

**Source:** `portfolio/state/portfolio.yaml` → `cash`

**Failure condition:** 
- `cash.balance` is negative
- `cash.free_tactical` ≠ `cash.balance` − `cash.strategic_reserve` − `cash.kelly_reserve` − `cash.dca_undeployed`

**Abort behavior:** Stop generation. Report: "Validation failed: cash sub-buckets do not reconcile with total balance."

---

### Check 6: Strategic Reserve Validation

**What it verifies:** SR value is valid and non-negative.

**Source:** `portfolio/state/portfolio.yaml` → `cash.strategic_reserve`

**Failure condition:** SR is negative or not a number.

**Abort behavior:** Stop generation. Report: "Validation failed: invalid Strategic Reserve value."

---

### Check 7: Kelly Reserve Validation

**What it verifies:** KR value is valid and non-negative.

**Source:** `portfolio/state/portfolio.yaml` → `cash.kelly_reserve`

**Failure condition:** KR is negative or not a number.

**Abort behavior:** Stop generation. Report: "Validation failed: invalid Kelly Reserve value."

---

### Check 8: Market Value Validation

**What it verifies:** Every position has a valid, positive market value.

**Source:** `portfolio/state/portfolio.yaml` → `positions[].market_value`

**Failure condition:** Any position has market_value ≤ 0 or market_value is not a number.

**Abort behavior:** Stop generation. Report: "Validation failed: invalid market value for [TICKER]."

---

### Check 9: Portfolio Total vs NLV Reconciliation

**What it verifies:** Authoritative NLV equals the sum of position market values plus cash (internal consistency). IBKR Reported NLV is reference only.

**Source:** `portfolio/state/portfolio.yaml` → `nlv`, `portfolio_total`, optional `ibkr_nlv`

**NLV policy (v2.1.2):**
- `nlv` MUST be set to `Σ(position.market_value) + cash.balance` (computed / real-time).
- `ibkr_nlv` (if present) is IBKR Reported NLV — reference only; API/time lag is expected.
- All LIOS calculations use `nlv`, never `ibkr_nlv`.

**Failure condition:** |nlv − portfolio_total| > $1.00. No exceptions. No soft-pass.

**Non-failure (INFO only):** |ibkr_nlv − nlv| may exceed $1.00. Log the gap in validation detail. Do not abort.

**Abort behavior:** Stop generation. Report: "RECONCILIATION FAILED. Delta: $X.XX. See reports/reconciliation.md." Do not generate any further reports until state is corrected and all 9 checks pass.

---

## Validation Log

Every successful or failed validation is recorded in `logs/validation/` with:
- Timestamp
- Snapshot reference
- NLV
- Portfolio total
- Reconciliation delta
- Validation status (PASSED / FAILED)
- Per-check results with details

## Agent Instructions

Before generating ANY report:
1. Load `portfolio/state/portfolio.yaml`
2. Run all 9 checks in order
3. If any check fails → abort, record in `logs/validation/`, report failure
4. If all checks pass → record in `logs/validation/`, proceed with generation

Never generate a report from an unvalidated or failed state.
