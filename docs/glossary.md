# LIOS Glossary (v7.0.0)

Terms used across LIOS docs. Sorted alphabetically.
Each entry: term, definition, where it's used.

> **Retired terms.** The following concepts were retired in earlier versions
> and are defined nowhere in live framework files: the numeric scoring
> layers and their component scores, the reserve concepts, the deleted
> sleeves, the replacement ranking, the old section-11 scoreboard, the old
> locked-decision numbering, the old trigger rules, the second denominator,
> and the discretionary channel. Their definitions live only in git history
> and the frozen `lios/` archives. No live instruction may reference them.

---

## A

### Amendment table
**Definition:** The single table in Part I (`constitution/constitution.md`) that classifies every kind of change as major, minor, or portfolio-local. It is the only amendment table in LIOS. No other file restates it.
**Used in:** `constitution/constitution.md`, `AGENTS.md`

### Agent role
**Definition:** Part VII (`core/agent-role.md`). The AI agent is the owner's advisor, reports to the owner, never trades, never commits or pushes without per-request approval, and never presents a draft framework as settled rules.
**Used in:** `core/agent-role.md`

---

## B

### Band (RI)
**Definition:** One of four Regime Index regimes: Risk-on 0–35 · Constructive 36–50 · Cautious 51–65 · Defensive 66–100. Total risky exposure is identical in all four. A band is a dashboard reading, not an instruction.
**Used in:** `core/health-check.md` §3

### Book
**Definition:** The IBKR portfolio — the whole book. Mox is operating cash outside the book; Mox securities are listed in the SSOT with their US-situs total so every cap is tested on the true figure.
**Used in:** `constitution/constitution.md` P1′, `portfolio/state/portfolio.yaml`

### Buying-pause
**Definition:** A cap hit on a Sleeve B name: stop buying it. If the name continues to run above the ceiling beyond tolerance, the quarterly review trims it to the ceiling.
**Used in:** `core/sleeves.md` Sleeve B

---

## C

### Cap (Sleeve B)
**Definition:** Maximum % of NLV a Sleeve B name may occupy (`position_cap_pct`, 5%). Caps are binding — both buying-pause and trim triggers. The ceiling (10% of NLV) is constitutional (P6′); the per-name cap is portfolio-local in the SSOT.
**Used in:** `constitution/constitution.md` P6′, `core/sleeves.md`

### Cap-only
**Definition:** A sleeve governed by a ceiling rather than a target weight. Applies to Sleeve B: no target, no glidepath share, no Top-up. The ceiling — 10% of NLV — binds at the quarterly review within the tolerance; between reviews a winner may run, with the overage reported every cycle.
**Used in:** `core/deployment.md`, `core/sleeves.md`

### Candidate
**Definition:** A proposed replacement for an incumbent Sleeve B holding. Compared head-to-head with the incumbent and with leaving the money in A on catalyst horizon, invalidation clarity, and look-through overlap.
**Used in:** `core/sleeves.md`, `constitution/method.md`

### Confidence %
**Definition:** Required on every recommendation in LIOS outputs. Format: `Confidence: XX% — [one-line rationale]`.
**Used in:** Communication standards, `constitution/principles.md`

### Contribution tilt
**Definition:** The glidepath policy. Targets govern new money only; equity is not sold to de-risk. Contrast with a hard glidepath, where targets are binding and Rebalance sells equity to reach them.
**Used in:** `core/glidepath.md`

---

## D

### DCA
**Definition:** Dollar-cost averaging. The monthly systematic deployment amount, a personal parameter in `portfolio/state/portfolio.yaml: monthly_dca`. Governed by the gap in the Top-up waterfall. The DCA amount changes only by a dated record in the SSOT.
**Used in:** `core/deployment.md` Top-up

### Drift
**Definition:** $ (or pp) by which a sleeve has moved from its target. In v7, gap and drift are the same number — one calculation, not two.
**Used in:** `commands/drift-check.md`, `core/health-check.md` §2

### Duration band
**Definition:** The single control inside Sleeve C. Average effective duration of 1–6 years, no single holding above 10y, holdings above 6y at or under 50% of C. Measured as a value-weighted average of fact-sheet effective duration; the average is a quarterly style guard, the single-holding cap is a hard breach. Report **money duration in dollars** with it — a 1pp rate move costs money duration × 1%. A barbell of bills and one long bond can satisfy the average while behaving in two directions, which is why the single-holding cap exists.
**Used in:** `constitution/constitution.md` P6′, `core/sleeves.md`, `core/validation.md` Check 11, Health Check §7

---

## E

### Effective exposure
**Definition:** A holding's direct weight plus its embedded weight in the index sleeves. A 7% direct position in a name that is 6.5% of a 65% core sleeve is about 11% effective. Single-name caps apply to this number, not to the direct weight.
**Used in:** `core/sleeves.md`, P6′

### Eligible new money
**Definition:** The month's DCA plus any arrears due that month plus any eligible capital injection pooled with that month's Top-up. Mox operating cash is excluded. `NLV' = NLV + eligible_new_money`.
**Used in:** `core/deployment.md` Top-up

---

## F

### Facts vs Assessments
**Definition:** Required separation in every LIOS output. Facts = observed/verifiable. Assessments = judgment calls with Confidence %.
**Used in:** Communication standards, all Health Check sections

### Funding Source
**Definition:** Where the capital for a sleeve deployment comes from. Sleeve B is funded only by an Investment Committee decision from Sleeve A.
**Used in:** Sleeve B satellites

---

## G

### Gap (G)
**Definition:** `T_A = risky_target × NLV' − B_actual_value`; `T_C = (1 − risky_target) × NLV'`; `G_A = T_A − A_value`, `G_C = T_C − C_value`; `G+ = max(G, 0)`. Positive = underweight. The only input to the Top-up waterfall other than target weights. Drift and gap are the same number.
**Used in:** `core/deployment.md`, `commands/drift-check.md`

### Governance Principle
**Definition:** "Rules are permanent. Decisions are dynamic." Constitutional layer of LIOS.
**Used in:** `constitution/constitution.md`

---

## H

### Health Check
**Definition:** The full portfolio report. Canonical output format. Always the same sections, always this order — **14 sections** (P8′). See `core/health-check.md`.
**Used in:** `core/health-check.md`, `commands/health-check.md`

---

## I

### IC (Investment Committee)
**Definition:** The owner + active AI session. Has sole authority on A → B funding decisions and on thesis-break reviews, may approve or reject any trade recommendation, may NOT modify a Portfolio Rule without a constitutional amendment.
**Used in:** `constitution/method.md`, `core/investment-committee.md`

### Incumbent
**Definition:** In a replacement evaluation, the current Sleeve B holding being evaluated.
**Used in:** `core/sleeves.md`

### Injection
**Definition:** Eligible capital moved into the book outside the monthly DCA — a bonus, a sale of an external holding, or another lump sum. An injection is pooled with the next Top-up and deployed pro-rata to positive gaps inside A. It is not a mandate and it is not a Top-up.
**Used in:** `core/deployment.md`, `constitution/method.md`

---

## L

### Look-through exposure
**Definition:** Exposure to a single company measured through every wrapper: direct holdings plus each fund's underlying weight in that company plus index-embedded weight. Two funds holding the same company are one exposure. Capped at 5% of NLV; single-sector exposure (25% of NLV) is measured the same way, including sector weight inside broad-index funds.
**Used in:** `constitution/constitution.md` P6′, `core/sleeves.md`, Health Check §10

---

## M

### Macro & Liquidity
**Definition:** Regime Index dimension #3, 20% weight. Rates, credit spreads, central-bank posture, USD, liquidity conditions. Higher = more hostile.
**Used in:** `core/health-check.md` §3

### Mandate (transition mandate)
**Definition:** P3a. A time-boxed structural migration. Requires eight things: sunset, completion criteria, fallback, cap compliance, window/window-year/actual-calendar-year cap, no overlap, minimum interval, and what a mandate may not do. At most three mandates raised in any actual calendar year, at least 90 days between raisings, sunset at most 92 days after raising. Windows are bounded by 31 January, 31 July, 31 October.
**Used in:** `constitution/constitution.md` P3a

### Mandate window
**Definition:** One of three windows per year: November–January, February–July, August–October. At most one mandate per window. A mandate window is a raising window, not a deadline.
**Used in:** `constitution/constitution.md` P3a

### Market Valuation
**Definition:** Regime Index dimension #1, 30% weight. Broad equity valuations — P/E, CAPE, equity risk premium, breadth. Higher = more hostile.
**Used in:** `core/health-check.md` §3

### Money duration
**Definition:** The dollar-weighted duration of Sleeve C: `Σ (MV_i × duration_i)`. A 1pp rate move costs money duration × 1%. Reported in dollars, never as an average alone.
**Used in:** `core/sleeves.md`, Health Check §7

---

## N

### NLV (Net Liquidation Value)
**Definition:** Total portfolio value. Gross Position Value + Cash. Always expressed in dollars, never just %.
**Used in:** Throughout LIOS

### NLV′ (NLV prime)
**Definition:** `NLV + eligible_new_money` — the denominator used by the Top-up formula. The month's contribution is included because the target grows with the money being deployed.
**Used in:** `core/deployment.md` Top-up

---

## O

### Opportunity Set
**Definition:** Regime Index dimension #5, 10% weight. Quality of buying opportunities available *now*. Higher = fewer opportunities.
**Used in:** `core/health-check.md` §3

---

## P

### P1′–P8′ (Portfolio Rules)
**Definition:** The eight constitutional rules as written in `constitution/constitution.md` Part I. P1′ three sleeves, one denominator, scope · P2′ targets from the glidepath, risky weight by age, Sleeve B cap-only for life · P3′ two operations only · P3a transition mandate · P4′ Rebalance direction limits · P5′ exits follow the instrument · P6′ caps outrank every rule · P7′ a cap breach is a Rebalance trigger · P8′ reporting. Changing any of them requires a major version bump plus an ADR.
**Used in:** `constitution/constitution.md`

### Per-sleeve band
**Definition:** The tolerance band for soft limits: `rebalance_tolerance_pct` (2.5pp). Soft-limit deviations inside the tolerance are reported, not traded; beyond it, the sleeve rebalances at the next quarterly review. Hard-cap breaches ignore the tolerance and trigger Rebalance immediately (P7′).
**Used in:** `core/deployment.md`

### Portfolio Valuation
**Definition:** Regime Index dimension #2, 25% weight. Valuation of the owner's specific holdings vs their own history, growth durability. Higher = more hostile.
**Used in:** `core/health-check.md` §3

---

## R

### Rebalance
**Definition:** The on-breach operation. Triggered **immediately** by a hard-cap breach; soft breaches (sleeve band or B ceiling beyond tolerance) wait for the quarterly review. Sells overweight and buys underweight, subject to P4′'s direction limits. This is the crash response.
**Used in:** `core/deployment.md` (P3′, P7′)

### Rebalance tolerance
**Definition:** `portfolio/state/portfolio.yaml: rebalance_tolerance_pct` (default 2.5pp). See Per-sleeve band.
**Used in:** `core/deployment.md`, `constitution/constitution.md` P3′, P7′

### Regime Index (RI)
**Definition:** 0–100 conditions gauge, higher = more hostile. Five weighted dimensions (Market Valuation 30 / Portfolio Valuation 25 / Macro & Liquidity 20 / Technical 15 / Opportunity 10). Dashboard only. It sizes nothing, shifts no target, gates no operation, and is not evidence for a thesis break or a replacement decision.
**Used in:** `core/health-check.md` §3, `commands/regime-recalc.md`

### Residual target
**Definition:** A target computed as the remainder, not stored. Sleeve C's target is `NLV − A − B`; Sleeve A's target is `risky_target × NLV' − B_actual`. A residual target is never a stored number in the SSOT.
**Used in:** `core/deployment.md`, `core/sleeves.md`

### Risky block
**Definition:** Sleeves A + B. Total risky weight = `(A + B) / NLV`. C is the defensive block. Counting C as risky is an error.
**Used in:** Throughout LIOS

---

## S

### Satellite Review
**Definition:** Health Check §11. For each B name: its written thesis, the evidence for and against it, and its review status — a list, explicitly **not a score** and not a ranking.
**Used in:** `core/health-check.md` §11

### Sector basis
**Definition:** The taxonomy a sector cap is computed on, recorded in `portfolio/state/portfolio.yaml: constraints.sector_basis` because "sector" is otherwise undefined and the same book is compliant or in hard breach depending on the reading. Default `gics_industry_group`. Broad GICS sector exposure is a dashboard metric, not a cap. A cap with no recorded basis is treated as non-compliant, not as passing.
**Used in:** `constitution/constitution.md` P6′, Health Check §10

### Sleeve
**Definition:** A capital layer in LIOS. Three sleeves: **A, B, C**. Every weight, cap and floor is a percentage of NLV. See `core/sleeves.md`.
**Used in:** `core/sleeves.md`

### Sleeve A (ETFs)
**Definition:** ETFs — broad-index **and** sector/thematic. Every holding carries a **view statement**: intended exposure, why a basket rather than a name, horizon, invalidation condition. A constituent's thesis break triggers a look-through review, not a sale of the fund.
**Used in:** `core/sleeves.md`, `constitution/constitution.md` P1′

### Sleeve B (Satellites, cap-only)
**Definition:** Individual companies and narrow bets. No target weight, no equal-weight rule, **no Top-up — ever**. Ceiling 10% of NLV, enforced at the quarterly review within the tolerance. Funded only by an Investment Committee decision from Sleeve A. Exits only on thesis break (P5′).
**Used in:** `core/sleeves.md`, `constitution/method.md`

### Sleeve C (Defensive)
**Definition:** Cash, money market, T-bills, and bond ETFs of any duration. **Residual target** — `C = NLV − A − B`; no stored target, no floor, no ceiling. Ballast and ammunition: the designated source for Rebalance into an underweight risky sleeve. **One control: average effective duration 1–6 years, no single holding above 10y, holdings above 6y at or under 50% of C.** Composition (cash vs bonds) is reported, never targeted. Bought, not rotated: no ranking.
**Used in:** `core/sleeves.md`, `constitution/constitution.md` P1′, P6′

### Snapshot
**Definition:** Date-stamped portfolio state. The canonical input for all LIOS commands.
**Used in:** `snapshots/`, `commands/snapshot.md`

### SSOT
**Definition:** Single source of truth — `portfolio/state/portfolio.yaml` in the private repo. Every number in a report traces to it.
**Used in:** Throughout LIOS

### Stop-adding line
**Definition:** $48,000 true US-situs exposure. At or above it, A's share of a Top-up buys only A's non-US-domiciled lines.
**Used in:** `constitution/constitution.md` P6′, `core/deployment.md`

---

## T

### Technical Conditions
**Definition:** Regime Index dimension #4, 15% weight. Trend, momentum, breadth, volatility regime, drawdown depth. Higher = more hostile.
**Used in:** `core/health-check.md` §3

### Thematic block
**Definition:** The sector/thematic holdings inside Sleeve A, taken together. Capped at 25% of Sleeve A. An A holding is either core beta or thematic; the split is recorded per position in the SSOT.
**Used in:** `core/sleeves.md`, `constitution/constitution.md` P6′

### Thesis break
**Definition:** The only exit trigger for a Sleeve B name (P5′). Established by documented research and verification, decided at the quarterly review. Price and drawdown are never exit triggers.
**Used in:** `constitution/constitution.md` P5′, `constitution/method.md`

### Top-up
**Definition:** The monthly operation. Allocates the contribution pro-rata to each sleeve's positive gap — computed over **A and C only** (B has no target and is never topped up). Never sells. Falls back to routing 100% to **C** when total risky weight is at the age-band cap. A's target is the residual `risky_target × NLV' − B_actual`.
**Used in:** `core/deployment.md` (P3′)

### Transition mandate
**Definition:** See Mandate.
**Used in:** `constitution/constitution.md` P3a

### Trim
**Definition:** A reduction of a Sleeve B name, permitted only at a Quarterly Review when B exceeds its ceiling beyond tolerance, and never below 1.0% of NLV.
**Used in:** `constitution/constitution.md` P5′, `core/sleeves.md`

### True US-situs
**Definition:** The US-situs exposure of the whole book plus listed external holdings (Mox securities), tested against the $60,000 ceiling and the $48,000 stop-adding line. The book figure alone is never the test.
**Used in:** `constitution/constitution.md` P6′, `core/validation.md` Checks 10 and 15

---

## U

### US-situs exposure
**Definition:** Assets subject to US estate tax for a non-US investor — US-domiciled ETFs and shares of US corporations. Irish-domiciled UCITS do not count. Tracked against a hard ceiling ($60,000) on the true figure. P6′.
**Used in:** `constitution/constitution.md` P6′, `portfolio/state/portfolio.yaml: constraints.us_situs_*`

---

## V

### View statement
**Definition:** The written justification required for every Sleeve A holding: intended exposure, why a basket rather than a single name, the horizon, and the condition that invalidates it. A thematic ETF is entered and reviewed as a view, not as a position in a company.
**Used in:** `core/sleeves.md`, `constitution/constitution.md` P1′

---

## W

### Watchlist
**Definition:** A thesis logged with no capital deployed. A watchlist entry is not a holding and does not consume Sleeve B ceiling headroom.
**Used in:** Sleeve B satellites

---

## Symbols

### Confidence: XX%
**Definition:** Required format. Number 0-100 + one-line rationale.

### Facts vs Assessments
**Definition:** Required separation. **Facts** = observed/verifiable. **Assessments** = judgment.

---

## Related

- See `constitution/constitution.md` for the canonical definitions in rule form
- See `core/sleeves.md`, `core/deployment.md`, `core/glidepath.md` for the operating definitions
