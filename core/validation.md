# LIOS v6 Auto-Validation Rules

**Version:** 5.0.0
**Last updated:** 2026-11-16

Every report generation runs these **12 checks** against
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

**Verifies:** every position is assigned to a valid v6 sleeve.

**Source:** `positions[].sleeve`

**Failure:** any sleeve value not in `{a, b, c}`. A `z` value is a v5-era file that was
not migrated — v6 has three sleeves.

**Abort:** "Validation failed: invalid sleeve assignment for [TICKER]."

**Note:** the v6 sleeve set is **A, B, C**. `sleeve: sr` is invalid —
the Strategic Reserve was repealed in v3.0.

---

### Check 5 · Sleeve Allocation and Risky Target

**Verifies:** sleeve allocation reconciles to NLV and the risky target matches the age band in `glidepath`.

**Source:** `positions[]`, `cash.balance`, `glidepath.bands` — **recomputed, not read**

**Failure:**
- sleeve totals recomputed from `positions[]` do not equal the stored `computed.sleeve_*` (±$1.00)
- recomputed NLV does not equal the stored `nlv` (±$1.00)
- A target does not equal `risky_target − B_actual`, or C target does not equal
  `NLV − risky_target` — both recomputed from holdings, never trusted from the stored block
- A target is **negative** (B above the whole risky budget): report it as an IC-level
  structural failure, not as a zero
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

**Source:** `regime.*` — **not** `bbi.*`. `bbi` was the v2.1 field; v6's Regime Index is a
dashboard (P2′), and no v6 SSOT is required to carry one. This check was pointing at a field
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
state is corrected and all 12 checks pass."

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

### Check 11 · Sleeve C Duration Band

**Verifies:** the defensive sleeve's duration is measured, and inside its band.

**Source:** `positions[].duration_years` for every position with `sleeve: c`;
`sleeves.c.duration_band` and `duration_single_max_years`.

**Failure:**
- any C position with a missing or non-numeric `duration_years`
- the value-weighted average outside `[duration_band.min_years, duration_band.max_years]`
- any single C holding above `duration_single_max_years` — this one is a **hard
  breach** (P7′), not a soft flag

**Abort:** "Validation failed: Sleeve C duration (avg X, band min–max)."

**Why this check exists.** v6 merged cash and bonds into one sleeve, so the sleeve's
risk is no longer implied by its name. "Defensive" is a role; duration is the
measurement. Without this check the sleeve is a label, and a label is exactly what a
future decision will lean on when it puts the whole sleeve in long duration.

---

### Check 12 · No Borrowing

**Verifies:** the book is not levered and cash is not negative.

**Source:** `cash.balance`, `Σ(positions[].market_value)`, `nlv`

**Failure:**
- `cash.balance < 0` — a margin debit, whatever the broker calls it
- `Σ(positions[].market_value) > nlv` beyond $1.00 (gross exposure above NLV)

**Abort:** "Validation failed: negative cash or gross exposure above NLV — the book is
levered. Do not generate reports or place orders."

**Why.** The account is margin-type so that unsettled proceeds can fund a same-session
buy. That is a settlement convenience, not permission to borrow: a buy leg larger than the
cash it is funded by silently converts a paired trade into leverage, duplicates the factor
exposure, and accrues interest. A negative cash balance is the only mechanical signal that
it happened.

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
2. Run all 12 checks in order
3. If any check fails → abort, record in `logs/validation/`, report the failure
4. If all pass → record, proceed

Never generate a report from an unvalidated or failed state.

---

## Removed in v3.0

| v2.x check | Fate |
|---|---|
| Strategic Reserve non-negative | Deleted — no SR exists |
| Kelly Reserve non-negative | Deleted — no KR exists |
| Cash sub-bucket reconciliation (SR/KR/TC/DCA) | Deleted — one cash balance, inside C |

Replaced by Check 5 (target consistency) and Check 6 (risk-factor purity), which
catch the class of error the old cash checks could not see.
