# `/sleeve-c-review` — Sleeve C Review

## Purpose

Review the defensive sleeve: composition, duration table, money duration,
and ammunition available for Rebalance. Minutes, no full report.

Sleeve C is the residual (C = NLV − A − B). It carries one control: average
duration 1–6 years, no single holding above 10y, holdings above 6y at or
under 50% of C. Composition is reported, never targeted.

## When to use

- Every monthly cycle, for the record
- Before any Rebalance that moves money through C
- When a fact sheet updates a holding's duration

## Inputs

1. `portfolio/state/portfolio.yaml`
2. Fact-sheet effective duration for each C holding (ISIN + date)

## Command

---

```
Review Sleeve C of my portfolio (LIOS v7).

State:
<paste state.yaml here>

Output:
1. Total, % of NLV, residual target and gap (G_C = T_C − C_value;
   T_C = (1 − risky_target) × NLV')
2. Composition: cash / money market / T-bills / bonds, in dollars and % of C
3. Duration table: every holding, its effective duration, its money duration,
   and the weighted average against the 1–6y band. Each holding's duration
   cites its fact-sheet source (ISIN + date)
4. Money duration in dollars, and what a 1pp rate move costs
5. Holdings with duration above 6 years as a share of C (cap 50%)
6. Ammunition available: the dollar amount C could deploy without breaching its band
7. Verdict: HOLD / ROTATE INSIDE C / REVIEW, with the rule cited

Rules:
- Composition is reported, never targeted. There is no cash/bond split target
- A barbell is not a loophole: if the composition is bills plus long bonds, say so
  explicitly, even when the average is inside the band
- A rotation inside C is not a transfer and is not tagged as a Rebalance (P4′)
- The fix for a duration breach is a rotation inside C, or a C → A Rebalance
  when C is overweight — never an A → C sale (prohibited)
- Cash renders as one row at duration 0
- No ranking inside C. C is bought, not rotated for performance

Confidence: XX% on the verdict.
```

---

## What you get back

A one-page Sleeve C review: totals, composition, duration table, money
duration, and a verdict with its rule citation.

## Related

- `../core/sleeves.md` — Sleeve C rules
- `../core/deployment.md` — rotation inside C vs Rebalance
- `health-check.md` §7 — where C appears in the 14-section report
