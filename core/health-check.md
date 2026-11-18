# Health Check Structure (14 Sections)

**LIOS v7.0.0 — adopted 2026-11-18. Supersedes v6.0.5 and every earlier version.**
**Status:** in force from adoption. The 14-section fixed order is constitutional (P8′); section content is method. Where this file disagrees with Part I (the constitution), Part I governs.

---

## Purpose

The Health Check is the canonical output format for a full session review. It is
the report run periodically to re-validate the portfolio against current
conditions.

**Always 14 sections, always this order.** Deviating breaks cross-session
comparability.

---

## The 14 Sections

| # | Section | Output |
|---|---|---|
| 1 | Portfolio Health | NLV, sleeve totals vs targets, reconciliation |
| 2 | Gap & Drift | One table — gap and drift are the same number |
| 3 | Regime Index | Five scores, weighted value, band (dashboard) |
| 4 | Constraint Check | Look-through name, sector, duration, risky, US-situs (true figure), B ceiling, B count, thematic block |
| 5 | Sleeve A Review | Holdings, view statements, within-A split, thematic drift — the A view-statement re-read |
| 6 | Sleeve B Review | Written theses, thesis-break status, ceiling status, name count |
| 7 | Sleeve C Review | Composition, duration table, money duration, band status, ammunition |
| 8 | Top-up Plan | Allocation in dollars (A, C — never B) |
| 9 | Rebalance Status | Triggered? Hard or soft? Executed? Escalated under P7′? |
| 10 | Look-Through Concentration | Name and sector exposure through all wrappers |
| 11 | Satellite Review | B names: thesis, evidence, review status — explicitly not a score |
| 12 | Execution Calendar | Earnings, macro, index events, mandate windows |
| 13 | Execution Plan | Buy/Hold/Reduce/Exit with dollars, each tagged to a P4′ direction and an operation |
| 14 | Risk Assessment & CIO Resolution | Drawdown ladder in dollars, then one paragraph |

---

## Section Specifications

### 1. Portfolio Health
- Total NLV, cash, sleeve-by-sleeve value and weight (% of NLV)
- Reconciliation delta (must be ≤ ±1.00)
- Three-line headline: where we are, what changed, what matters next
- Headline **facts**, not assessments

### 2. Gap & Drift
- One table: sleeve, value, target dollars, gap dollars, gap in pp, band status
- `T_A = risky_target × NLV' − B_actual_value`; `T_C = (1 − risky_target) × NLV'`; `G_A = T_A − A_value`, `G_C = T_C − C_value`; `G+ = max(G, 0)`; `NLV' = NLV + eligible_new_money`
- **Positive gap = underweight = fundable.** State the sign convention on the line.
  A wrong sign is silent: a negative in an underweight sleeve reads as a valid number and
  allocates zero
- **B appears in the table with a ceiling, not a target.** Its "gap" is the distance
  to the ceiling, and a positive distance is not fundable.
- Σ gaps over {A, C} must equal zero
- Band: sleeve bands within `rebalance_tolerance_pct` (2.5pp). A soft breach beyond
  tolerance triggers Rebalance **at the quarterly review**
- **Express gaps in dollars first, percentages second**

### 3. Regime Index — dashboard
- Five dimension scores, each 0–100, each with a one-line rationale citing a specific data point. Higher means more hostile on every dimension: Market Valuation (0 = cheap against history, 100 = expensive); Portfolio Valuation (0 = cheap against its history, 100 = expensive); Macro & Liquidity (0 = loose and ample, 100 = tight and restrictive); Technical (0 = strong uptrend, 100 = breakdown); Opportunity (0 = many attractive opportunities, 100 = few or none)
- Weighted total: `RI = 0.30·MarketVal + 0.25·PortfolioVal + 0.20·MacroLiq + 0.15·Technical + 0.10·Opportunity`
- Band: Risk-on 0–35 · Constructive 36–50 · Cautious 51–65 · Defensive 66–100
- Band change versus the prior cycle — reported for context only
- **The index has no mechanical effect.** It sizes nothing, shifts no target, and
  gates no operation. It is not evidence for a thesis break or a replacement decision. State this explicitly each cycle
- If a dimension cannot be scored from available data, say so — do not extrapolate

### 4. Constraint Check
- Look-through single-company exposure (direct + fund look-through + index-embedded), with the fund holding weights and their dates recorded so a second session can reproduce the computation
- Single-sector exposure, including sector weight inside broad-index funds, **on the
  basis recorded in `constraints.sector_basis`** (GICS industry group) — report the basis on the same line
  as the number, and if it is not recorded, report the constraint as
  `NON-COMPLIANT: basis undefined`, never as passing
- Duration (Sleeve **C**): average 1–6y, no single holding above 10y, holdings above 6y at or under 50% of C
- Total risky weight = `(A + B) / NLV`
- **True US-situs exposure** against the $60,000 ceiling and the $48,000 stop-adding line, with the book figure reported beside it
- Sleeve B against its ceiling (10–12.5% reported, not trimmed; above 12.5% at the review trimmed) and against `names_max`
- Thematic block share of Sleeve A against `sleeves.a.thematic_max_pct` (25%)
- **State each limit in dollars, not only in percent**

### 5. Sleeve A Review — the A view-statement re-read
- Per holding: dollars, % of A, % of NLV
- **Within-A split: core beta versus thematic** against the thematic cap
- For each holding: the written view statement, its horizon, and whether
  the invalidation condition has been met — re-read against the quarter's evidence (Part V, agenda item 7)
- **Look-through note:** which single companies the sleeve owns across all funds,
  and the largest of them
- Verdict per holding: HOLD / RESIZE / REVIEW / EXIT, with the P5′ clause cited
- A holding whose view is invalidated is reviewed under P5′; exiting a single thematic holding on a P5′ trigger is a Rebalance (A→A), with proceeds to the most underweight A line

### 6. Sleeve B Review
- Per holding: dollars, % NLV, written thesis and its invalidation condition, days since last review
- **Ceiling status:** B vs `sleeves.b.ceiling_pct`, and the dollar overage if any
- **Thesis-break status:** any open thesis-break review, its deadline date, and whether second-session verification is recorded
- **Name count** against `names_max`, and each name against `position_cap_pct`
- Note explicitly that B received **$0.00** from this month's Top-up
- No score governs any decision in this section. There is no ranking, no weakest name, no composite

### 7. Sleeve C Review — defensive, and one sleeve
- Total, % NLV, residual target and gap
- **Composition**: cash / bills / bonds, in dollars and % of C — reported, never targeted
- **Duration table**: every holding, its effective duration, its money duration, and
  the weighted average against the 1–6y band; each holding's duration cites its fact-sheet source (ISIN + date)
- **Money duration in dollars**, and what a 1pp rate move costs — an average without
  dollars hides the stake
- Holdings with duration above 6 years as a share of C (cap 50%)
- **Ammunition available**: the dollar amount C could deploy without breaching its band
- **A barbell is not a loophole**: if the composition is bills plus long bonds, say so
  explicitly, even when the average is inside the band

### 8. Top-up Plan
- `NLV' = NLV + eligible_new_money` (the month's DCA + any arrears due that month + any eligible capital injection pooled with that month's Top-up)
- Allocation per sleeve in **dollars**, across A and C only, pro-rata to positive gaps; if `risky_now` is at or above the risky cap (risky target + tolerance), the whole contribution goes to C
- State `risky_now = (A + B) / NLV` and whether the risky-cap branch was taken
- State the true US-situs figure against the $48,000 stop-adding line: at or above it, A's share buys only A's non-US-domiciled lines
- **State that Sleeve B receives $0.00 by rule**

### 9. Rebalance Status
- Any hard-cap breach: which one, how large in dollars, cure plan, and the P7′ 10-business-day deadline (cured means the trade filled)
- Any soft breach: which sleeve, how far beyond tolerance, and whether the quarterly
  review is due
- Whether a triggered Rebalance executed a trade or not
- Escalation status under P7′: any accepted breach, its expiry date, and whether the acceptance has lapsed or is exhausted

### 10. Look-Through Concentration
- Top 10 underlying companies across the entire book, in dollars and % of NLV
- Each against the look-through cap; a value between 4.5% and 5% is `AT CAP — no further addition`
- Sector totals against the sector cap, **stating the basis** (GICS industry group) and the measurement error you are working within — a hand-built
  look-through has one; recorded fund holding weights older than 120 days fail the check
- Broad GICS sector totals as a **dashboard metric, not a cap** — reported, never
  enforced
- **Name the double-counting risk explicitly:** which companies appear in more than
  one wrapper, and the combined weight

### 11. Satellite Review
- For each B name: its written thesis, the evidence for and against it, and its review status — a list, explicitly **not a score** and not a ranking
- Thesis-break reviews: open items with deadline dates, verification verdicts, and the owner's confirmation
- Whether any candidate has IC approval to be funded from A, with the six-part IC record
- Name count against `names_max`, and each name against `position_cap_pct`; if the count is above the limit, state the grandfathering position and its re-decision date — never a forced or comparative sale
- Replacement candidates, where any: compared head-to-head with the incumbent and with leaving the money in A on catalyst horizon, invalidation clarity, and look-through overlap

### 12. Execution Calendar
- Earnings dates for B names
- Index rebalance and fund distribution dates for A holdings
- Macro events and scheduled reviews
- The next quarterly review date (last Saturday of February, May, August, November)
- Mandate window check: is a mandate open, is the current window free, is the next window boundary (31 January, 31 July, 31 October) approaching

### 13. Execution Plan
- Per line: Buy / Hold / Reduce / Exit, with dollars and share counts
- Every action traces to a rule: band, cap, Top-up, or mandate
- Every sale is tagged with its P4′ direction (C→A, B→A, inside C, A→A, A→B) and its operation (Rebalance, mandate leg); an untagged sale, an A→C sale, or a trim below the 1.0% of NLV floor is a breach
- An action with no rule citation is deleted from the plan

### 14. Risk Assessment & CIO Resolution
- Drawdown ladder in dollars against the owner's stated tolerance
- Factor and concentration risk in dollars
- Then one paragraph: the decision, the reasoning, the confidence %

---

## Cross-Section Discipline

- **Conclusions first.** State the finding, then the evidence
- **Dollars always.** A percentage alone is not an answer
- **Facts separate from assessments.** Mark which is which
- **No orphan sections.** Every section gets content; "no action" plus one line is
  acceptable
- **No score governs any decision.** The Regime Index in section 3 is a dashboard only; the confidence percentage is a reporting standard, not evidence

---

## Related

- `constitution/constitution.md` — Part I (P1′–P8′ + P3a)
- `constitution/method.md` — Part V (quarterly review, thesis breaks, reporting)
- `core/deployment.md` — Top-up, Rebalance, tolerance
- `core/sleeves.md` — sleeve definitions and constraints
- `core/validation.md` — the 30 checks, including the trade ledger (Check 30)
- `core/agent-role.md` — Part VII (agent role and guardrails)
- `templates/HealthCheck.md` — the fill-in template
- `commands/health-check.md` — the paste-ready prompt
