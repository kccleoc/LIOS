# `/drift-check` — Gap & Drift Report

## Purpose

Report the distance between each sleeve and its target. In v3.0 **gap and drift
are the same number** — there is one calculation, not two.

## When to use

- Monthly, before the DCA
- After any trade or large price move
- Before invoking Rebalance

## Inputs

1. `portfolio/state/portfolio.yaml`
2. Sleeve targets from the glidepath band for the current age
3. Per-sleeve bands: clamp(0.5 × target, 2pp, 8pp)

## Command

---

```
Produce the LIOS v4.1 gap / drift report.

State:
<paste state.yaml here>

One denominator only: NLV. There is no ABC base, no Strategic Reserve line,
no Kelly Reserve line.

For each sleeve in A, B, C, D, Z:
  value ($ and % of NLV)
  target ($ and % of NLV)
  gap ($) = target $ − value $
  gap (pp) = target % − actual %
  band status: inside / breach (± the rebalance band)

Then:
1. Confirm Σ gaps = 0
2. Name every sleeve that breaches the band
3. If any breach: state that Rebalance is triggered and give the proceed sizes
   (sell overweight to target, buy underweight pro-rata, Z never below floor)
4. Within-A weight drift against the within-A allocation
5. Within-B slot drift against the equal-weight slot bands

Rules:
- Dollars first, percentages second
- Gap and drift are the same number — do not compute them twice
- Definitions: a 0–1y Treasury instrument is Z, never A or D
- Explicit directive per sleeve: no action / flag / rebalance
- Confidence % on the rebalance recommendation

Begin.
```

---

## Definitions

```
G_s = w_s × (NLV + DCA) − V_s
```

Positive = underweight. Negative = overweight.

| Sleeve | Definition |
|---|---|
| A | Broad-index ETFs only |
| B | Individual equities, 1–4, equal weight at entry |
| C | Satellite / rotation, winner-weighted, ceiling 7% |
| D | Duration > 3y, hedged |
| Z | Duration ≈ 0, floor 3%, ceiling 8% |

## Thresholds

| Gap | Action |
|---|---|
| Inside the per-sleeve band | none |
| Band breach | Rebalance |
| Cap breach (P7) | Rebalance, or escalate if P4 forbids the cure |
| D +25% relative to target | Rebalance |

## Related

- `../core/deployment.md`
- `../core/sleeves.md`
- `../commands/health-check.md` §2
