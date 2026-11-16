> **Source:** Extracted from LIOS v1.5, rebuilt for three sleeves in v6. P8′ — the 14-section fixed order.

# Health Check Structure (15 Sections)

**Component version:** v5
**Status:** Constitutional (P8′) for structure; method for content
**Last updated:** 2026-11-16

---

## Purpose

The Health Check is the canonical output format for a full session review. It is
the report run periodically to re-validate the portfolio against current
conditions.

**Always 14 sections, always this order.** Deviating breaks cross-session
comparability.

**Why 15 and not 16.** v4.1 had a separate Sleeve D Review. v5 deletes sleeve D and
moves duration into C, so the "ballast and ammunition" section is now C's. A section
that reports an empty sleeve is a section nobody reads.

---

## The 14 Sections

| # | Section | Output |
|---|---|---|
| 1 | Portfolio Health | NLV, sleeve totals vs targets, reconciliation |
| 2 | Gap & Drift | One table — gap and drift are the same number |
| 3 | Regime Index | Five scores, weighted value, band (dashboard) |
| 4 | Constraint Check | Look-through name, sector, duration, risky, US-situs |
| 5 | Sleeve A Review | Holdings, view statements, within-A split, thematic drift |
| 6 | Sleeve B Review | LOS, thesis-exit status, ceiling status, replacement ranking |
| 7 | Sleeve C Review | Composition, duration table, money duration, band status, ammunition |
| 8 | Top-up Plan | Allocation in dollars (A, C — never B) |
| 9 | Rebalance Status | Triggered? Hard or soft? Executed? Escalated under P7′? |
| 10 | Look-Through Concentration | Name and sector exposure through all wrappers |
| 11 | Satellite Scoreboard | B names by RI, plus replacement candidates |
| 12 | Execution Calendar | Earnings, macro, index events |
| 13 | Execution Plan | Buy/Hold/Reduce/Exit with dollars |
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
- `G_s = w_s × NLV − V_s`. Gap and drift are the same number; never compute both
- **Positive gap = underweight = fundable.** State the sign convention on the line.
  A wrong sign is silent: `−$6,098` in an underweight sleeve reads as a valid number and
  allocates zero
- **B appears in the table with a ceiling, not a target.** Its "gap" is the distance
  to the ceiling, and a positive distance is not fundable.
- Σ gaps over {A, C} must equal zero
- Band: per sleeve, `clamp(0.5 × target, 2pp, 8pp)`. Soft breach beyond
  `rebalance_tolerance_pct` triggers Rebalance **at the quarterly review**
- **Express gaps in dollars first, percentages second**

### 3. Regime Index — dashboard
- Five dimension scores, each with a one-line rationale citing a data point
- Weighted total: `0.30·MV + 0.25·PV + 0.20·ML + 0.15·T + 0.10·O`
- Band: Risk-on 0–35 · Constructive 36–50 · Cautious 51–65 · Defensive 66–100
- Band change versus the prior cycle — reported for context only
- **The index has no mechanical effect.** It sizes nothing, shifts no target, and
  gates no operation. State this explicitly each cycle

### 4. Constraint Check
- Look-through single-company exposure (direct + fund look-through + index-embedded)
- Single-sector exposure, including sector weight inside broad-index funds, **on the
  basis recorded in `constraints.sector_basis`** — report the basis on the same line
  as the number, and if it is not recorded, report the constraint as
  `NON-COMPLIANT: basis undefined`, never as passing
- Duration (Sleeve **C**)
- Total risky weight = `(A + B) / NLV`
- US-situs exposure against the ceiling, with the 80% early warning
- Sleeve B against its ceiling
- **State each limit in dollars, not only in percent**

### 5. Sleeve A Review
- Per holding: dollars, % of A, % of NLV
- **Within-A split: core beta versus thematic** against the thematic cap
- For each thematic holding: the written view statement, its horizon, and whether
  the invalidation condition has been met
- **Look-through note:** which single companies the sleeve owns across all funds,
  and the largest of them
- Verdict per holding: HOLD / RESIZE / REVIEW / EXIT, with the P5′ clause cited

### 6. Sleeve B Review
- Per holding: dollars, % NLV, LOS and its four components, days since last score
- **Ceiling status:** B vs `sleeves.b.ceiling_pct`, and the dollar overage if any
- **Thesis-exit status:** count of names below LOS 60 and any open 30-day review
- **Replacement ranking** by Replacement Index; name the weakest
- Note explicitly that B received **$0.00** from this month's Top-up

### 7. Sleeve C Review — defensive, and one sleeve
- Total, % NLV, residual target and gap
- **Composition**: cash / bills / bonds, in dollars and % of C — reported, never targeted
- **Duration table**: every holding, its effective duration, its money duration, and
  the weighted average against the 1–6y band
- **Money duration in dollars**, and what a 1pp rate move costs — an average without
  dollars hides the stake
- Currency hedge ratio, credit quality, and the fact-sheet date each duration came from
- **Ammunition available**: the dollar amount C could deploy without breaching its band
- **A barbell is not a loophole**: if the composition is bills plus long bonds, say so
  explicitly, even when the average is inside the band

### 8. Top-up Plan
- `NLV' = NLV + DCA`
- Allocation per sleeve in **dollars**, across A and C only
- State `risky_now = (A + B) / NLV` and whether the risky-cap branch was taken
- **State that Sleeve B receives $0.00 by rule**

### 10. Rebalance Status
- Any hard-cap breach: which one, how large in dollars, cure plan
- Any soft breach: which sleeve, how far beyond tolerance, and whether the quarterly
  review is due
- Whether a triggered Rebalance executed a trade or not
- Escalation status under P7′ if a breach cannot be cured within the P4′ limits

### 11. Look-Through Concentration
- Top 10 underlying companies across the entire book, in dollars and % of NLV
- Each against the look-through cap
- Sector totals against the sector cap, **stating the taxonomy** (industry group by
  default) and the measurement error you are working within — a hand-built
  look-through has one
- Broad GICS sector totals as a **dashboard metric, not a cap** — reported, never
  enforced
- **Name the double-counting risk explicitly:** which companies appear in more than
  one wrapper, and the combined weight

### 12. Satellite Scoreboard
- B names ranked by Replacement Index, with the weakest flagged
- Watchlist candidates with LOS and LFS
- The replacement test: composite superiority plus two of four qualitative conditions
- Whether any candidate has IC approval to be funded from A
- Name count against `names_max`, and each name against `position_cap_pct`

### 13. Execution Calendar
- Earnings dates for B names
- Index rebalance and fund distribution dates for A holdings
- Macro events and scheduled reviews
- The next quarterly Rebalance review date

### 14. Execution Plan
- Per line: Buy / Hold / Reduce / Exit, with dollars and share counts
- Every action traces to a rule: band, cap, Top-up, or mandate
- An action with no rule citation is deleted from the plan

### 15. Risk Assessment & CIO Resolution
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
- **No Kelly Factor, no Strategic Reserve, no `ABC` denominator.** If a section
  needs one, the section is wrong
- **No Sleeve D.** If a section references it, the section is stale

---

## Related

- `core/deployment.md` — Top-up, Rebalance, tolerance
- `core/regime-index.md` — Regime Index
- `core/sleeves.md` — sleeve definitions and constraints
- `core/replacement-index.md` — B replacement test
- `templates/HealthCheck.md` — the fill-in template
- `commands/health-check.md` — the paste-ready prompt
