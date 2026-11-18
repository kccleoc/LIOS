# Sleeves (Part III)

**LIOS v7.0.0 — adopted 2026-11-18. Supersedes v6.0.5 and every earlier version.**
**Status:** in force from adoption. This file is part of the v7.0.0 framework; where it disagrees with Part I (the constitution), Part I governs.

Three sleeves on one denominator (NLV). A + B is the risky block; C is the defensive block and the residual.

---

## Part III — Sleeves

```
NLV = A + B + C
```

| Sleeve | Name | Holds | Risk role |
|---|---|---|---|
| **A** | Conviction ETFs | Broad-index **and** sector/thematic ETFs | Systematic and thematic beta |
| **B** | Satellites | Individual companies and narrow bets | Idiosyncratic alpha, **cap-only** |
| **C** | Defensive | Cash, money market, bills, bond ETFs of any duration | Ballast and ammunition, **residual** |

**A + B = the risky block. C = the defensive block and the residual.**

Every weight, cap and floor is a percentage of NLV. No second denominator. Nothing is defined by an absolute target except the risky total: A and C are both derived from it.

**Instrument taxonomy.** Every position carries an instrument type, recorded in the SSOT and used by Check 6:

| Type | Meaning |
|---|---|
| Broad-index ETF | A diversified basket tracking a broad market index |
| Sector/thematic ETF | A basket tracking one sector or theme |
| Narrow/single-theme ETP | An instrument whose risk is concentrated in one company or one narrow theme |
| Individual equity | A single company's shares |
| Cash/bills/bond ETF | Cash, money market, bills, or a bond fund of any duration |

A position's sleeve must match its type: a broad-index ETF or a sector/thematic ETF belongs in A; a narrow/single-theme ETP or an individual equity belongs in B; a cash/bills/bond ETF belongs in C. (See Check 6.)

### Sleeve A — Conviction ETFs

**Target:** residual — `A_target = risky_target × NLV − B_actual_value` (dollars). A target is never entered by hand.

**Contents:** ETFs. Two roles inside one sleeve:

| Role | Within-A target | Notes |
|---|---|---|
| Core beta — broad-index equity funds | 50% and 30% (per-line within-A targets recorded in the SSOT) | Residual after thematic |
| Thematic block — sector or theme baskets (positions with `thematic_block: true`) | **20% combined** | The block's carrier and members are identified by the `thematic_block` attribute in the SSOT; block members have no separate targets |

**The thematic block is one target, not three positions.** The block's combined within-A target is 20%. Within the block, an allocated dollar is split **pro-rata to the block's existing composition** (positions with `thematic_block: true`), matching C's rule — no line inside the block is chosen by discretion. Block members carry no separate targets because they are members of the block, and a block member is **not** a 0% holding: the dust rule does not apply to it (see below). The thematic share of A is capped at 25% of A (`thematic_max_pct`, hard; SSOT field `sleeves.a.thematic_max_pct`; tested by Check 23) so that a sector view can never become the portfolio's dominant exposure by drift.

**Rules:**

- Every A holding carries a **written view statement**: intended exposure, why the basket rather than a name, horizon, and the condition that invalidates it.
- A thematic ETF is entered and reviewed as a *view*, not as a *position in a company*. A constituent's management failure does not invalidate the view.
- **Look-through concentration is measured fund by fund** (P6′). Two funds holding the same company are one exposure.
- No cash-like instrument sits inside A. A 0–1y Treasury fund is C.
- A is never sold to fund C (P4′).
- Rotating the thematic BLOCK (changing the block's allocation) requires a transition mandate. Exiting a single thematic ETF on a P5′ trigger is a Rebalance (A→A), with proceeds to the most underweight A line. A rotation inside A is a Rebalance, not a discretionary trade.
- **The dust rule.** A holding at a 0% target **outside a defined block** has no claim on capital. A position listed at `target_pct: 0` that still holds value is an unfinished exit, not a position. It receives no contribution and is dispositioned at the next Rebalance, proceeds to the A line most underweight. A member of a defined block (the thematic block) is not a 0% holding and the dust rule does not touch it. Without this rule a residual-target formula — which can only ever allocate *to A as a whole* — leaves an unfinished exit funded forever.

### Sleeve B — Satellites (cap-only)

**Ceiling:** 10% of NLV, at every age, for life. **Target:** none. B has no target weight.
**Structure:** individual companies and narrow bets. No equal-weight rule.
**Concentration inside B:** `names_max` (5 names) and `position_cap_pct` (5% of NLV per name). `position_cap_pct` is a hard test on every report. `names_max` is a **forward cap**: it binds additions, not incumbents — no new name above the limit, and no forced sale of an existing one to make a count come out. Both are reported every cycle. The block on the next addition is immediate and cannot be deferred; a count already above the limit is a soft item settled at the Quarterly Review (Part V), and it never justifies a sale made only to bring the number down.

**The B count comes down by decision, not by arithmetic.** When the count must fall, the choice is which name — on its thesis — and the proceeds go to an IC decision or to A. Selling the smallest position because it is smallest is the dust rule applied to a name that is not dust.

**Count reduction (6 → 5).** When the count is above `names_max` and must fall, each B holding is tested **solely against its own written thesis and invalidation condition** (P5′). Only a holding whose own thesis is broken by cited, owner-verified evidence is exited — there is no comparative ranking, no "weakest thesis", and no forced sale. If all six theses remain intact, the count is **grandfathered**: it is reported as an accepted soft breach under P7′ with a re-decision date at the next quarterly review, and re-decided every quarter until the count falls or the constitution is amended. Grandfathering is a deferral, not a permanent exemption. *(Owner-confirmed 2026-11-18: this mechanism decides the B-count question at the 2027-02-27 review by grandfathering unless a thesis genuinely breaks — see Open item 8.)*

**Funding:**

- **B receives no Top-up. Ever.** The monthly contribution never touches B.
- B is funded only by an **Investment Committee decision** from Sleeve A, and only while B is below its ceiling.
- B is never funded from C.

**B funding must come from A's pre-existing holdings, not from money that arrived in the same cycle.** Because A's target is residual, an A→B transfer is gap-neutral — it leaves A's dollar gap untouched — so new money routed into A could otherwise be transferred straight into B, funding B with contributions while nominally obeying "B is never topped up". To close that: an A→B funding transfer may not be approved within **30 calendar days of a capital injection**; the IC record for any A→B transfer must state the **source of the transferred dollars** (the sixth IC-record part, Part V); and the principle is that B funding comes from A's pre-existing holdings. Money is fungible, so this is a dated-record plus 30-day friction control, not a perfect seal — it is stated as such. The 30-day bar covers capital injections only and cannot extend to the monthly DCA: a DCA lands every month, so the bar would prohibit A→B transfers permanently. The control for DCA-to-B is the IC decision plus the B caps (10% ceiling, 5% name, 5 names) — those are the binding controls, and they are what stop contributions reaching B at scale.

**Two jobs B does:**

1. **Hold the owner's conviction, within the caps.** A B name is a single-company bet the owner has researched and written a thesis for. The caps — not a score — are what keep it small.
2. **Let a winner run, within a soft ceiling.** B is not trimmed the moment a name appreciates past the ceiling. The ceiling enforces at the **quarterly Rebalance review**, within the tolerance band. Between reviews it may exceed the ceiling — that is reported every cycle, not silently tolerated.

**Selling:**

- **Trim** — above the ceiling beyond tolerance, at the quarterly review. Trim the **largest** position; trims reduce concentration and are not exits.
- **Exit** — only on a thesis break (P5′). The break is established by documented research and verification, recorded in the SSOT note for the position, and decided at the quarterly review.

**Rules:**

- No satellite position exceeds `position_cap_pct` of NLV.
- Effective exposure counts index-embedded weight (P6′).
- New names require Investment Committee approval and a written thesis. **Replacement choice is by IC judgment on the written thesis: at least two alternatives must be considered and recorded, compared head-to-head with the candidate on three structured dimensions — catalyst horizon, invalidation clarity, and look-through overlap with existing holdings** — for a replacement, the candidate against the incumbent it would replace and against leaving the money in A (see Part V).
- US-situs holdings count toward the estate-tax ceiling (P6′).

### Sleeve C — Defensive (cash, bills, bonds)

**Target:** **residual** — `C = NLV − A − B`. No stored target, no floor, no ceiling. C's Top-up target is the derived complement `(1 − risky_target) × NLV'`, used only to compute the gap (Part II); C has no independently-set target and receives whatever the Top-up does not allocate to A.
**Contents:** cash, money market, T-bills, 0–1y funds, and bond ETFs of any duration. Anything that is not equity beta and settles in a day.

**Rules:**

- **One sleeve, one control: the duration band.** Average effective duration 1–6 years; no single holding above 10 years. The average is tested at the quarterly review and is a soft limit, cured there by rotation inside C; the single-holding cap is a hard breach, and holdings above 6 years exceeding 50% of C is a hard breach — both cured within 10 business days by rotation inside C (P7′).
- **The average includes cash.** Cash is a C holding at 0 years, so the denominator is the whole sleeve.
- **Long holdings are capped by value, not by money-duration share.** Holdings with duration above **6 years** may not exceed **50% of C's value**. The average alone hides the stake: half of C in a 10-year bond passes the 1–6y average (5.1y) and the 10-year per-holding cap, while putting a large rate position at risk. Value share is the quantity that distinguishes "a bond allocation" from "a duration bet".
- **No internal distribution.** There is no cash target, no bond target, no split between them, and no sub-sleeve. Composition is a measurement, not an allocation rule. But "no target" is not "no instruction": a contribution that reaches C buys **pro-rata to C's existing composition** (cash if C is empty), so the average stays where it was instead of the owner inventing a bond/cash split every month.
- **Sales out of C are pro-rata**, never shortest-first. Draining the bills to buy equities in a crash would spike the average toward the bond line and breach the ceiling in the middle of the event the sleeve exists for.
- C is the designated source for Rebalance into an underweight risky sleeve, and that is the permitted de-cashing direction (P4′).
- C never funds B directly under P4′; it funds A, and A may fund B only by IC decision.
- **Rotation inside C is not a transfer.** Moving from one defensive instrument to another stays inside the sleeve and needs no direction permission; it is how the duration band is cured.
- C hedges growth and deflation shocks. **It does not hedge inflation shocks** — in an inflation-driven selloff duration falls alongside equities. That is why the average is capped and why a barbell is not a loophole.
- C is bought, not rotated for performance. There is no ranking, no replacement score, no winner-weighting.
- The emergency fund sits **outside NLV**. C is not an emergency fund.

### Constraints (P6′)

| # | Constraint | Parameter |
|---|---|---|
| 1 | Look-through single-company exposure | `constraints.lookthrough_company_cap_pct` |
| 2 | Single-sector exposure | `constraints.factor_cap_pct` |
| 3 | Sleeve C duration (avg 1–6y, none > 10y, >6y holdings ≤ 50% of C) | `sleeves.c.duration_*` |
| 4 | Total risky weight | `glidepath.bands` |
| 5 | US-situs assets | `constraints.us_situs_ceiling` |
| 6 | Sleeve B ceiling | `sleeves.b.ceiling_pct` |
| 7 | Sleeve B name count and single-name cap | `sleeves.b.names_max`, `sleeves.b.position_cap_pct` |

A target that violates a constraint is invalid and must be re-solved. When a constraint and a rule conflict, **the constraint wins** (P6′), and a hard breach is itself a Rebalance trigger (P7′).

**Look-through accounting.** A fund is a bundle, not a single exposure. Exposure to a company is the sum of direct holding plus every fund's weight in that company plus index-embedded weight. The cap is applied to that sum. **The Health Check records the inputs** — fund holding weights and their dates — so a second session can reproduce the computation and disagree (Part VI).

**US-situs early warning:** stop adding at 80% of the ceiling and prefer non-US-domiciled instruments where the exposure is otherwise identical. A capital-neutral swap between two US-situs instruments leaves US-situs unchanged and is not restricted by this line.

### Cross-sleeve invariants

1. One denominator: NLV.
2. Caps are caps, not targets.
3. Only two operations move money (P3′).
4. A sleeve ceiling never sits above its band and calls itself enforcement.
5. Cash is a sleeve, not a mood.
6. Satellites are funded by decision, never by contribution.

### Funding matrix

| From ↓ / To → | A | B | C |
|---|---|---|---|
| Top-up | ✅ | ❌ **never** | ✅ |
| Rebalance | ✅ | ✅ **IC only** | ✅ (only out of C, into A) |
| Capital injection | ✅ (via the gap formula, capped at the A gap) | ❌ **never** | ✅ (remainder) |
| **Prohibited** | — | A→B automatic | **A → C** |

Rotation **inside** C is not a transfer and needs no row: moving between two defensive instruments is how the duration band is cured. A Rebalance that corrects a band or cap breach never funds B: the correction goes to A (C→A) or stays inside its sleeve. The only route into B is an IC A→B transfer from A's pre-existing holdings (Part III, above), executed as a Rebalance leg on its own decision, never as the destination of a correction.

---
