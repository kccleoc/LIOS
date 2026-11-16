# `/drift-check` — Gap & Drift Report

## Purpose

Report the distance between each sleeve and its target. In v6, **gap and drift are
the same number** — there is one calculation, not two.

Sleeve B is the exception: it has a **ceiling, not a target**, so it has no gap to
fund. Its line reports distance to the ceiling.

## When to use

- Monthly, before the DCA
- Quarterly, as the Rebalance review
- After any trade or large price move
- Before invoking Rebalance

## Inputs

1. `portfolio/state/portfolio.yaml`
2. Sleeve targets from the glidepath band for the current age
3. Per-sleeve bands: `clamp(0.5 × target, 2pp, 8pp)`, and the tolerance from
   `state.yaml: rebalance_tolerance_pct`
4. Look-through holdings data for the funds in Sleeve A

## Command

---

```
Produce the LIOS v6 gap / drift report.

State:
<paste state.yaml here>

One denominator only: NLV. There are three sleeves: A, B, C. There is no
Sleeve D, no ABC base, no Strategic Reserve line, no Kelly Reserve line.

For each sleeve in A, B, C:
  value ($ and % of NLV)
  target ($ and % of NLV)     — for B, report the CEILING instead of a target
  gap ($) = target $ − value $
  gap (pp) = target % − actual %
  band status: inside / beyond tolerance / breach

Then:
1. Confirm Σ gaps over {A, C} = 0
2. Compute risky weight as (A + B) / NLV — NOT (A + B + C). C is bonds.
3. Name every sleeve whose deviation exceeds the tolerance band
4. State whether each trigger is HARD (immediate) or SOFT (next quarterly review)
5. If a hard breach: give the cure in dollars, subject to the P4′ direction limits
6. If a soft breach: state the dollar size and the date of the next quarterly review
7. Within-A: core-beta vs thematic split against the thematic cap
8. Within-A: look-through top-10 companies in dollars and % of NLV, each against
   the look-through cap
9. Sleeve B: ceiling status only, and state that B received $0.00 from Top-up

Rules:
- Dollars first, percentages second
- Gap and drift are the same number — do not compute them twice
- B has no target. A positive distance to its ceiling is NOT fundable.
- A 0–1y Treasury instrument is C, never A
- Explicit directive per sleeve: no action / flag / rebalance now / rebalance at review
- Confidence % on the rebalance recommendation

Begin.
```

---

## Definitions

```
G_s = w_s × (NLV + DCA) − V_s      for s in {A, C}
```

Positive = underweight. Negative = overweight. **B is excluded** — it has no target.

| Sleeve | Definition | Limit |
|---|---|---|
| A | ETFs, broad-index **and** thematic | target band |
| B | Satellites — individual companies | **ceiling only** |
| C | Duration > 3y, hedged | target band |
| C | cash / bills / bonds | residual target; duration 1–6y average |

## Thresholds

| Condition | Type | Action |
|---|---|---|
| Inside tolerance (`rebalance_tolerance_pct`, default 2.5pp) | none | report only |
| Soft breach beyond tolerance | SOFT | Rebalance at the next quarterly review |
| Look-through company or sector above cap | **HARD** | Rebalance now |
| US-situs above ceiling | **HARD** | Rebalance now |
| Z below floor | **HARD** | Rebalance now |
| Sleeve B above ceiling beyond tolerance | SOFT | Trim to ceiling at the next review |

## Related

- `../core/deployment.md`
- `../core/sleeves.md`
- `../commands/health-check.md` §2
