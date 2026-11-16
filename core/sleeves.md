> **Source:** Replaces the v4.1 five-sleeve architecture. Governed by P1′, P2′, P6′.

# The Four Sleeves — A / B / C

**Component version:** v5
**Status:** Constitutional (P1′, P2′, P6′)
**Last updated:** 2026-11-15

---

## Structure

```
NLV = A + B + C
```

| Sleeve | Name | Holds | Risk role |
|---|---|---|---|
| **A** | Conviction ETFs | Broad-index **and** sector/thematic ETFs | Systematic and thematic beta |
| **B** | Satellites | Individual companies and narrow bets | Idiosyncratic alpha, **cap-only** |
| **C** | Defensive | Cash, money market, bills, bond ETFs of any duration | Ballast and ammunition, **residual** |

**A + B = the risky block. C = the defensive block and the residual.**

Every weight, cap and floor is a percentage of NLV. No second denominator. Nothing
is defined by an absolute target except the risky total: A and C are both residual.

---

## Sleeve A — Conviction ETFs

**Target:** `state.yaml: sleeves.a.target_pct`
**Contents:** ETFs. Two roles inside one sleeve:

| Role | Example | Within-A band |
|---|---|---|
| Core beta | broad-index equity fund | residual after thematic |
| Thematic | sector or theme basket | `sleeves.a.thematic_max_pct` |

**Rules:**

- Every A holding carries a **written view statement**: intended exposure, why the
  basket rather than a name, horizon, and the condition that invalidates it.
- A thematic ETF is entered and reviewed as a *view*, not as a *position in a
  company*. A constituent's management failure does not invalidate the view.
- **Look-through concentration is measured fund by fund** (P6′). Two funds holding
  the same company are one exposure.
- No cash-like instrument sits inside A. A 0–1y Treasury fund is C.
- A is never sold to fund C (P4′).
- A rotation of a thematic ETF is permitted under the Transition Mandate rules, and
  only then. A rotation inside A is a Rebalance, not a discretionary trade.
- **A holding at a 0% target has no claim on capital.** A position listed at
  `target_pct: 0` that still holds value is an unfinished exit, not a position. It
  receives no contribution and is dispositioned at the next Rebalance, proceeds to
  the A line most underweight. Without this rule a residual-target formula — which
  can only ever allocate *to A as a whole* — leaves such a holding funded forever.

**Thematic ceiling.** The thematic share of A is capped so that a sector view can
never become the portfolio's dominant exposure by drift. The cap lives in
`state.yaml` because it is portfolio-local.

---

## Sleeve B — Satellites (cap-only)

**Ceiling:** `state.yaml: sleeves.b.ceiling_pct` — **10% of NLV**
**Target:** none. B has no target weight.
**Structure:** individual companies and narrow bets. No equal-weight rule.
**Concentration inside B:** `sleeves.b.names_max` (default 5 names) and
`sleeves.b.position_cap_pct` (default 5% of NLV per name).
`position_cap_pct` is a hard test on every report. `names_max` is a **forward cap**: it
binds additions, not incumbents — no new name above the limit, and no forced sale of an
existing one to make a count come out. Both are reported every cycle.

**Funding:**

- **B receives no Top-up. Ever.** The monthly contribution never touches B.
- B is funded only by an **Investment Committee decision** from Sleeve A, and only
  while B is below its ceiling.
- B is never funded from C.

**Two jobs B does:**

1. **Replace the weakest.** The weakest holding by Replacement Index is a standing
   candidate for replacement by the next qualifying candidate. Replacement requires
   composite superiority plus two of four qualitative conditions
   (`constitution/method.md`).
2. **Let a winner run, within a soft ceiling.** B is not trimmed the moment a name
   appreciates past the ceiling. The ceiling enforces at the **quarterly Rebalance
   review**, within the tolerance band. Between reviews it may exceed the ceiling —
   that is reported every cycle, not silently tolerated.

**Selling:**

- **Trim** — above the ceiling beyond tolerance, at the quarterly review. Trim the
  **largest** position; trims reduce concentration and are not exits.
- **Exit** — only on a thesis break (P5′). LOS < 60 opens a 30-day deep-dive review
  that must resolve to a documented re-score ≥ 60 or exit. No third outcome.

**Rules:**

- No satellite position exceeds `sleeves.b.position_cap_pct` of NLV.
- Effective exposure counts index-embedded weight (P6′).
- New names require Investment Committee approval and LOS ≥ 80.
- US-situs holdings count toward the estate-tax ceiling (P6′).

---

## Sleeve C — Defensive (cash, bills, bonds)

**Target:** **residual** — `C = NLV − A − B`. No stored target, no floor, no ceiling.
**Contents:** cash, money market, T-bills, 0–1y funds, and bond ETFs of any
duration. Anything that is not equity beta and settles in a day.

**Rules:**

- **One sleeve, one control: the duration band.** Average effective duration 1–6
  years; no single holding above 10 years. The average is tested at the quarterly
  review; the single-holding cap is a hard breach.
- **The average includes cash.** Cash is a C holding at 0 years, so the denominator is
  the whole sleeve. Excluding it would lift the same book from 1.30y to 1.42y and quietly
  move the band test.
- **Long holdings are capped by value, not by money-duration share.** Holdings with
  duration above **6 years** may not exceed **50% of C's value**. The average alone hides
  the stake: half of C in a 10-year bond passes the 1–6y average (5.1y) and the 10-year
  per-holding cap, while putting $2,444 at risk per 1pp on a $47k sleeve. A money-duration
  share was the first attempt and was rejected: it forbids any sleeve that holds a single
  bond fund — 74.5% of this sleeve's money duration sits in one 15% position, which is
  correct behaviour, not a breach. Value share is the quantity that distinguishes "a bond
  allocation" from "a duration bet".
- **No internal distribution.** There is no cash target, no bond target, no split
  between them, and no sub-sleeve. Composition is a measurement, not an allocation
  rule. But "no target" is not "no instruction": a contribution that reaches C buys
  **pro-rata to C's existing composition** (cash if C is empty), so the average stays
  where it was instead of the owner inventing a bond/cash split every month.
- **Sales out of C are pro-rata**, never shortest-first. Draining the bills to buy
  equities in a crash would spike the average toward the bond line and breach the ceiling
  in the middle of the event the sleeve exists for. The v5 split into a bond sleeve and a cash sleeve generated most of that
  version's defects and changed no behaviour: a T-bill ETF and a bond ETF are both
  T+1 sellable and both non-risky in role.
- C is the designated source for Rebalance into an underweight risky sleeve, and
  that is the permitted de-cashing direction (P4′).
- C never funds B directly under P4′; it funds A, and A may fund B only by IC
  decision.
- **Rotation inside C is not a transfer.** Moving from one defensive instrument to
  another stays inside the sleeve and needs no direction permission; it is how the
  duration band is cured.
- C hedges growth and deflation shocks. **It does not hedge inflation shocks** — in
  an inflation-driven selloff duration falls alongside equities. That is why the
  average is capped and why a barbell is not a loophole.
- C is bought, not rotated for performance. There is no ranking, no Replacement
  Index, no winner-weighting. Those rules are deleted with the v4.1 satellite
  sleeve.
- The emergency fund sits **outside NLV**. C is not an emergency fund.



---

## Constraints (P6′)

| # | Constraint | Parameter |
|---|---|---|
| 1 | Look-through single-company exposure | `constraints.lookthrough_company_cap_pct` |
| 2 | Single-sector exposure | `constraints.factor_cap_pct` |
| 3 | Sleeve C duration (avg 1–6y, none > 10y) | `sleeves.c.duration_*` |
| 4 | Total risky weight | `glidepath.bands` |
| 5 | US-situs assets | `constraints.us_situs_ceiling` |
| 6 | Sleeve B ceiling | `sleeves.b.ceiling_pct` |

A target that violates a constraint is invalid and must be re-solved. When a
constraint and a rule conflict, **the constraint wins** (P6′), and a hard breach is
itself a Rebalance trigger (P7′).

**Look-through accounting.** A fund is a bundle, not a single exposure. Exposure to
a company is the sum of direct holding plus every fund's weight in that company
plus index-embedded weight. The cap is applied to that sum.

**US-situs early warning:** stop adding at 80% of the ceiling and prefer
non-US-domiciled instruments where the exposure is otherwise identical. A
capital-neutral swap between two US-situs instruments leaves US-situs unchanged
and is not restricted by this line.

---

## Cross-sleeve invariants

1. One denominator: NLV.
2. Caps are caps, not targets.
3. Only two operations move money (P3′).
4. A sleeve ceiling never sits above its band and calls itself enforcement.
5. Cash is a sleeve, not a mood.
6. Satellites are funded by decision, never by contribution.

---

## Funding matrix

| From ↓ / To → | A | B | C |
|---|---|---|---|
| Top-up | ✅ | ❌ **never** | ✅ |
| Rebalance | ✅ | ✅ **IC only** | ✅ (only out of C, into A) |
| **Prohibited** | — | A→B automatic | **A → C** |

Rotation **inside** C is not a transfer and needs no row: moving between two
defensive instruments is how the duration band is cured.

---

## Deprecated

| Concept | Fate |
|---|---|
| `ABC` denominator | Deleted (v3.0) |
| Strategic Reserve | Deleted (v3.0) |
| Kelly Reserve | Deleted (v3.0) |
| Free tactical bucket | Deleted (v3.0) |
| **Sleeve D** | **Deleted (v5)** — duration role absorbed by C |
| **Satellite rotation / winner-weighting** | **Deleted (v5)** |
| **Single-name position-level caps** | **Replaced by look-through caps (v5)** |
| **Equal weight at entry** | **Deleted (v5)** — B is cap-only |

---

See also: `core/deployment.md`, `core/glidepath.md`, `core/regime-index.md`,
`constitution/constitution.md`.
