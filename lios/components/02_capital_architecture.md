> **Source:** Replaces the v4.1 five-sleeve architecture. Governed by P1′, P2′, P6′.
> Sleeve D is deleted in v5; its duration role is absorbed by C.

# 02 · Capital Architecture — Four Sleeves A/B/C

**Component version:** v6
**Status:** Constitutional (P1′, P2′, P6′)
**Last updated:** 2026-11-15

---

## Purpose

All capital sits in exactly three sleeves, measured against **one denominator:
NLV**.

```
NLV = A + B + C
```

| Sleeve | Name | Holds | Risk role |
|---|---|---|---|
| **A** | ETFs | Broad-index **and** sector/thematic ETFs | Systematic and thematic beta |
| **B** | Satellites | Individual companies and narrow bets — **cap-only, ceiling 10% of NLV** | Idiosyncratic alpha |
| **C** | Duration | Bonds, duration > 3 years, hedged | Ballast and ammunition |
| **C** | Defensive | Cash, money market, bills, bond ETFs of any duration | Ballast and ammunition, **residual** |

**A + B = the risky block. C = the defensive block, and the residual.**

Every weight, cap and floor is a percentage of NLV. No second denominator
exists. Drift and gap are the same number.

---

## Sleeve A — ETFs

**Target:** `state.yaml: sleeves.a.target_pct` — the residual,
`risky_target − B_actual` (P2′)
**Contents:** ETFs only, in two roles:

| Role | Holds | Within-A limit |
|---|---|---|
| Core beta | broad-index equity fund | residual after thematic |
| Thematic | sector or theme basket | `sleeves.a.thematic_max_pct` |

**A is where a view is expressed through a basket.** A sector or thematic ETF is a
legitimate A holding provided it carries a **written view statement**: the intended
exposure, why a basket rather than a company, the horizon, and the condition that
invalidates it.

**Rules:**

- A thematic ETF is entered and reviewed as a *view*, not as a *position in a
  company*. A constituent's management failure does not invalidate the view — it
  triggers a look-through review (P5′).
- A is never sold to fund C. That is de-risking, and de-risking is the
  glidepath's job (P4′).
- A rotation inside A is a Rebalance, and permitted in full only under a Transition
  Mandate (P3a).
- Look-through concentration is measured fund by fund. Two funds holding the same
  company are one exposure (P6′).
- No cash-like instrument sits inside A. A 0–1y Treasury fund is C.
- **A→B is an Investment Committee decision, not a Rebalance output.** B has no
  target, so it has no gap to fund automatically.

---

## Sleeve B — Satellites (cap-only)

**Ceiling:** `state.yaml: sleeves.b.ceiling_pct` — **10% of NLV**
**Target:** none. B has no target weight and no band.
**Structure:** individual companies and narrow bets. No equal-weight rule at entry.

**Funding:**

- **B receives no Top-up. Ever.** The monthly contribution never touches B.
- B is funded only by an **Investment Committee decision from Sleeve A**, and only
  while B sits below its ceiling. The addition is gap-neutral: A's target is the
  residual, so the money comes out of A's own gap, not out of A's target.
- B is never funded from C (P4′).

**Two jobs B does:**

1. **Replace the weakest.** The weakest holding by Replacement Index is the standing
   candidate for replacement by the next qualifying name. Replacement requires
   composite superiority plus two of the four qualitative conditions
   (`04_replacement_index.md`) and is **size-neutral**: one out, one in, same dollars.
2. **Let a winner run, within a ceiling.** A satellite that appreciates past the
   ceiling is not trimmed on sight. The ceiling enforces at the **quarterly Rebalance
   review**, within the tolerance band; between reviews it may exceed the ceiling and
   is reported every cycle.

**Selling:**

- **Trim** — above the ceiling beyond tolerance, at the quarterly review. Trim the
  **largest** position. A trim is risk control and is not an exit.
- **Exit** — only on a thesis break (P5′). LOS < 60 opens a 30-day deep-dive review
  that must resolve to a documented re-score of 60 or above citing new evidence, or
  to exit. No third outcome. An unfinished review exits the position.

**Rules:**

- No satellite exceeds `sleeves.b.position_cap_pct` of NLV.
- Effective exposure counts index-embedded weight (P6′).
- New names require Investment Committee approval and LOS ≥ 80.
- Price decline, drawdown, valuation and macro conditions are never exit triggers.
- US-situs holdings count toward the estate-tax ceiling (P6′).

---

## Sleeve C — Defensive (cash, bills, bonds)

**Target:** **residual** — `C = NLV − A − B`. No stored target, no floor, no ceiling.
**Contents:** cash, money market, T-bills, 0–1y funds, bond ETFs of any duration.

**Rules:**

- **One control: the duration band.** Average effective duration 1–6 years; no single
  holding above 10 years. The average is a quarterly style guard, the single-holding
  cap is a hard breach.
- **No internal distribution.** No cash target, no bond target, no sub-sleeve.
  Composition is reported, never targeted. The v5 split into a bond sleeve and a
  cash sleeve generated most of that version's defects and changed no behaviour.
- C has two jobs: ballast, and ammunition. It is the designated source for a
  Rebalance into an underweight risky sleeve.
- C never funds B directly. It funds A, and A may reach B only by Investment
  Committee decision (P4′).
- C hedges growth and deflation shocks. It does **not** hedge inflation shocks — in
  an inflation-driven selloff duration falls alongside equities. That is accepted,
  and is why long duration is not the only defensive holding.
- C is bought, not ranked. There is no Replacement Index, no winner-weighting and no
  laggard cut list inside C. Those rules govern B.

---

**The emergency fund sits outside NLV.** C is not an emergency fund. Rotation
inside C is not a transfer — it is how the duration band is cured.

---

## Why the denominator changed

Pre-v3.0 measured the core and satellite sleeves against `ABC = NLV − D − SR` while
measuring single-name caps against NLV. One dollar carried two weights. The
resulting gap widened when it was funded, because new cash entered the denominator
as well as the numerator, and no deployment rule could close it.

v3.0 deleted ABC. Drift and gap became the same number, and v5 keeps it that way:
one denominator, NLV, for every weight, cap and floor.

---

## Constraint Set

| # | Constraint | Parameter |
|---|---|---|
| 1 | Look-through single-company exposure | `constraints.lookthrough_company_cap_pct` |
| 2 | Single-sector exposure | `constraints.factor_cap_pct` |
| 3 | Duration sleeve (C) | `sleeves.c.target_pct` |
| 4 | Total risky weight, `(A + B) / NLV` | `glidepath.bands` |
| 5 | US-situs assets | `constraints.us_situs_ceiling` |
| 6 | Sleeve B ceiling | `sleeves.b.ceiling_pct` |
| 7 | Sleeve C duration band | `sleeves.c.duration_*` |

Targets are outputs of constraints. A target that violates a constraint is invalid
and must be re-solved. **The cap wins** (P6′), and a hard breach is itself a
Rebalance trigger (P7′).

---

## Removed

| Concept | Fate |
|---|---|
| `ABC` denominator | Deleted (v3.0) |
| Strategic Reserve | Repealed (v3.0) |
| Kelly Reserve | Deleted (v3.0) |
| Free tactical bucket | Deleted (v3.0) |
| Cash-like ETF inside A | Prohibited |
| **Sleeve D** | **Deleted (v5)** — the duration role is absorbed by C |
| **Five-sleeve architecture** | **Deleted (v5)** — three sleeves |
| **"Satellite" as a rotating, winner-weighted sleeve** | **Deleted (v5)** — that was C, and C is bonds |
| **Equal weight at entry in B** | **Deleted (v5)** — B is cap-only |
| **Single-name position-level cap in place of look-through** | **Replaced (v5)** by look-through caps |

---

See also: `core/sleeves.md`, `core/deployment.md`, `core/glidepath.md`,
`constitution/constitution.md`, `04_replacement_index.md`.
