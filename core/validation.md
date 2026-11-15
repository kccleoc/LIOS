# LIOS v5 Auto-Validation Rules

**Version:** 5.0.0
**Last updated:** 2026-11-16

Every report generation runs these **10 checks** against
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

**Exception:** a position carrying `status: pending` is a **target, not a holding** — it is
in `positions[]` only so the mandate can name it. Skip this check for it. Without the
exception, a post-mandate SSOT aborts report generation by construction, and a pipeline that
cannot pass is a pipeline nobody runs.

**Abort:** "Validation failed: invalid share count for [TICKER]."

---

### Check 3 · Average Cost

**Verifies:** every position has a positive, numeric average cost.

**Source:** `positions[].avg_cost`

**Failure:** any `avg_cost ≤ 0`, or not a number.

**Exception:** `status: pending` — see Check 2.

**Abort:** "Validation failed: invalid average cost for [TICKER]."

---

### Check 4 · Sleeve Assignment

**Verifies:** every position is assigned to a valid v5 sleeve.

**Source:** `positions[].sleeve`

**Failure:** any sleeve value not in `{a, b, c, z}`.

**Abort:** "Validation failed: invalid sleeve assignment for [TICKER]."

**Note:** the v5 sleeve set is **A, B, C, Z**. `sleeve: sr` is invalid —
the Strategic Reserve was repealed in v3.0.

---

### Check 5 · Sleeve Allocation and Risky Target

**Verifies:** sleeve allocation reconciles to NLV and the risky target matches the age band in `glidepath`.

**Source:** `sleeves.a.target_pct`, `sleeves.b.ceiling_pct`, `sleeves.c.target_pct`,
`sleeves.z.*`, `glidepath.bands`

**Failure:**
- sleeve totals do not sum to 100% of NLV (±0.1)
- A target does not equal the risky target minus actual B weight
- risky target does not match the glidepath band for the investor's age

**Abort:** "Validation failed: sleeve allocation or risky target is inconsistent."

---

### Check 6 · Sleeve Risk-Factor Purity

**Verifies:** each sleeve holds instruments consistent with its definition.

**Source:** `positions[]` joined with instrument metadata

**Failure:**
- a cash-like instrument (duration < 1y) assigned to sleeve `a` or `c`
- a duration-bearing instrument (duration > 3y) assigned to sleeve `z`
- an individual equity assigned to sleeve `a`
- a broad-index ETF assigned to sleeve `b`

**Abort:** "Validation failed: [TICKER] violates the risk-factor definition of
sleeve [X]."

**Note:** this is the check that prevents the v2.x error of booking a 0–1y
Treasury fund as a bond sleeve. Duration is the test, not the issuer.

---

### Check 7 · Regime Index (dashboard, optional)

**Verifies:** *if* the state carries a Regime Index block, it is internally consistent.

**Source:** `regime.*` — **not** `bbi.*`. `bbi` was the v2.1 field; v5's Regime Index is a
dashboard (P2′), and no v5 SSOT is required to carry one. This check was pointing at a field
that no longer exists, so it could not run at all.

**Failure:**
- any dimension missing, or outside 0–100
- `weighted` does not equal `0.30·MV + 0.25·PV + 0.20·ML + 0.15·T + 0.10·O`
  (within 0.1)
- `band` does not match the weighted value
  (0–35 Risk-On / 36–50 Constructive / 51–65 Cautious / 66–100 Defensive)
- more than one band step versus the prior cycle

**Abort:** none. If the block is absent, report `Regime Index: not computed` and continue.
If present and inconsistent, report it as a dashboard discrepancy — never abort report
generation on a metric that governs no rule.

**Note:** a large move in the index is reported as a **change worth noticing**, not as a
breach. This note previously called a second band step "a rule violation (P2)", which
contradicted `core/regime-index.md`: the index has no mechanical effect and gates nothing.
A dashboard cannot be violated.

---

### Check 8 · Market Value

**Verifies:** every position has a valid, positive market value.

**Source:** `positions[].market_value`

**Failure:** any `market_value ≤ 0`, or not a number.

**Exception:** `status: pending` — see Check 2.

**Abort:** "Validation failed: invalid market value for [TICKER]."

---

### Check 9 · NLV Reconciliation

**Verifies:** authoritative NLV equals the sum of position market values plus
cash.

**Source:** `nlv`, `portfolio_total`, optional `ibkr_nlv`

**NLV policy (P1′):**
- `nlv` must equal `Σ(positions[].market_value) + cash.balance` — computed
- `ibkr_nlv`, if present, is broker-reported and is **reference only**
- every LIOS calculation uses `nlv`, never `ibkr_nlv`
- there is one denominator. The former `ABC` denominator is not used.

**Failure:** `|nlv − portfolio_total| > $1.00`. No exceptions, no soft pass.

**Non-failure (INFO only):** `|ibkr_nlv − nlv|` may exceed $1.00. Log the gap, do
not abort.

**Abort:** "RECONCILIATION FAILED. Delta: $X.XX. Do not generate reports until
state is corrected and all 10 checks pass."

---

### Check 10 · US-situs Derivation

**Verifies:** the stored US-situs figure is derived, not asserted.

**Source:** `constraints.us_situs_exposure`, `positions[].us_situs`, `cash.balance`,
`constraints.us_situs_includes_cash`

**Rule:** `us_situs_exposure` must equal
`Σ(positions where us_situs: true).market_value` plus `cash.balance` if
`us_situs_includes_cash` is true.

**Failure:** the stored figure differs from the derivation by more than $1.00.

**Abort:** "Validation failed: US-situs derivation mismatch (stored X, derived Y)."

**Why this check exists.** This field was wrong for weeks by a whole *definition*, not by a
rounding error: it omitted every line whose domicile flag was incorrect, and the omission was
exactly one sleeve. Nothing caught it, because the only reconciliation in the pipeline tested
the total that every other number is derived from. A hand-kept total with no derivation check
is the one number in the file that no other number constrains.

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
2. Run all 10 checks in order
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
