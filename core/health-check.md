> **Source:** Extracted from LIOS v1.5, rebuilt for the five-sleeve architecture in v3.0. P8 — the 16-section fixed order.

# Health Check Structure (16 Sections)

**Component version:** v3.0
**Status:** Constitutional (P8) for structure; method for content
**Last updated:** 2026-11-14

---

## Purpose

The Health Check is the canonical output format for a full session review. It is
the report run periodically to re-validate the portfolio against current
conditions.

**Always 16 sections, always this order.** Deviating breaks cross-session
comparability.

---

## The 16 Sections

| # | Section | Output |
|---|---|---|
| 1 | Portfolio Health | NLV, sleeve totals vs targets, reconciliation |
| 2 | Gap & Drift | One table — gap and drift are the same number |
| 3 | Regime Index | Five scores, weighted value, band (dashboard) |
| 4 | Constraint Check | Single name, factor, duration, risky, US-situs |
| 5 | Sleeve A Review | Holdings + within-A weight drift |
| 6 | Sleeve B Review | LOS, thesis-exit status, slot drift vs band |
| 7 | Sleeve C Review | Rotation ranking, ceiling status, laggard list |
| 8 | Sleeve D Review | Duration, currency hedge, ammunition |
| 9 | Sleeve Z Review | Floor and ceiling, vehicle policy |
| 10 | Top-up Plan | Allocation in dollars |
| 11 | Rebalance Status | Triggered? Executed? Escalated under P7? |
| 12 | Frontier Scoreboard | C names + candidates |
| 13 | Execution Calendar | Earnings, macro, index events |
| 14 | Execution Plan | Buy/Hold/Reduce/Exit with dollars |
| 15 | Risk Assessment | Constraints + drawdown ladder in dollars |
| 16 | CIO Resolution | One paragraph, confidence % |

---

## Section Specifications

### 1. Portfolio Health
- Total NLV, cash, sleeve-by-sleeve value and weight (% of NLV)
- Reconciliation delta (must be ≤ ±1.00)
- Three-line headline: where we are, what changed, what matters next
- Headline **facts**, not assessments

### 2. Gap & Drift
- One table: sleeve, value, target dollars, gap dollars, gap in pp, band status
- `G_s = w_s × NLV − V_s`. Gap and drift are the same number; never compute both
- Σ gaps must equal zero
- Band: per sleeve, `clamp(0.5 × target, 2pp, 8pp)`. A breach triggers Rebalance
- **Express gaps in dollars first, percentages second**

### 3. Regime Index — dashboard
- Five dimension scores, each with a one-line rationale citing a data point
- Weighted total: `0.30·MV + 0.25·PV + 0.20·ML + 0.15·T + 0.10·O`
- Band: Risk-on 0–35 · Constructive 36–50 · Cautious 51–65 · Defensive 66–100
- Band change versus the prior cycle — reported for context only
- **The index has no mechanical effect.** It sizes nothing, shifts no target,
  and gates no operation. State this explicitly each cycle

### 4. Constraint Check
- Single-name **effective** exposure (direct + index-embedded)
- Single-factor exposure
- Duration (Sleeve D)
- Total risky weight versus the age band
- US-situs assets versus the ceiling
- Satellite ceiling
- Report limit / actual / status. Breaches are reported, never fatal

### 5. Sleeve A Review
- Each holding: shares, market value, weight within A, drift from target weight
- Any ticker on `state.yaml: sleeves.a.permanently_exited` → flag; do not re-enter
- **No cash-like instrument may appear in A.** A 0–1y Treasury fund is Z, not A
- A vehicles (broad-index ETFs) are not LOS- or LFS-scored — state this explicitly

### 6. Sleeve B Review
- Each holding: shares, market value, % of NLV, % of B, slot band status
- LOS beside each position, and LFS beside LOS
- **Thesis-exit status is mandatory**: state explicitly whether any name is below
  LOS 60. LOS < 60 is a mandatory exit (P5), not a discussion item
- Effective exposure versus the single-name cap
- Directive per name: Hold / Exit (thesis) — never a price-based action

### 7. Sleeve C Review
- Each holding: market value, % of NLV, % of C, LOS, Replacement Index
- Relative strength versus the sleeve benchmark (6m, 12m)
- Ceiling status (default 7%). Above ceiling → trim the **weakest** name
- Laggard list: bottom half for two consecutive quarters → cut or justify in
  writing
- Rotation proposals that clear the Replacement Index

### 8. Sleeve D Review
- Duration, currency of issue and hedge status
- Percentage of NLV versus target
- Ammunition available for Rebalance (how much D is above target, in dollars)
- D never funds B or C — restate if a proposal implies otherwise

### 9. Sleeve Z Review
- Value, % of NLV, floor (3%) and ceiling (8%)
- Vehicle policy: duration ≈ 0 only. No uninvested cash beyond policy
- A short-dated Treasury ETF is cash, not a hedge. Restate if challenged

### 10. Top-up Plan
- Show `NLV'`, `G_s` per sleeve, `ΣG⁺`, and the allocation
- Table: sleeve, allocation, share of positive gap
- State whether the risky-cap fallback applies (whole contribution to D and Z)
- If Rebalance is triggered and executes trades, state that Top-up is not run
  separately that month

### 11. Rebalance Status
- Not triggered / triggered and executed / triggered but no legal move (then
  escalate under P7)
- Which sleeves or caps breached, and by how much
- Direction of any trade, and confirmation it obeyed P4
- Any unresolved breach, escalated to the Investment Committee

### 12. Frontier Scoreboard
- All Sleeve C names plus watchlist candidates
- Replacement Index composite, LOS, trend
- Any candidate that clears the bar

### 13. Execution Calendar
- Upcoming earnings for B and C holdings
- Macro events in the next 30 days
- Index events, ex-dividend dates, scheduled reviews

### 14. Execution Plan
- BUY / HOLD / REDUCE / EXIT with dollar amounts
- Confidence % and one-line rationale per action
- Every action traces to Top-up or Rebalance. There is no third path.

### 15. Risk Assessment
- Constraint breaches
- Concentration by name and factor
- The drawdown ladder **in dollars**: ordinary bad quarter, bad quarter, bad
  year, severe drawdown
- If a ladder number is unacceptable, the fix is the glidepath — not a market call

### 16. CIO Resolution
- One paragraph
- Conclusions first, dollars, confidence %
- Ends with a single directive

---

## Cross-Section Discipline

- **Conclusions first.** State the finding, then the evidence
- **Dollars always.** A percentage alone is not an answer
- **Facts separate from assessments.** Mark which is which
- **No orphan sections.** Every section gets content; "no action" plus one line
  is acceptable
- **No Kelly Factor, no Strategic Reserve, no `ABC` denominator.** If a section
  needs one, the section is wrong

---

## Related

- `core/deployment.md` — the three money rules
- `core/regime-index.md` — Regime Index
- `core/sleeves.md` — sleeve definitions and constraints
- `templates/HealthCheck.md` — the fill-in template
- `commands/health-check.md` — the paste-ready prompt
