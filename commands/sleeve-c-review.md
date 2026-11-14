# `/sleeve-c-review` — Satellite Rotation Review

## Purpose

Review Sleeve C: winners, laggards, ceiling status, and whether any name should
be cut.

## When to use

- Quarterly (the review cadence for C)
- Before any rotation or new name
- After a material move in a C holding

## Inputs

1. `portfolio/state/portfolio.yaml`
2. Current LOS / Replacement Index scores for each C name
3. Sleeve benchmark price series (for relative-strength ranking)

## Command

---

```
Run a Sleeve C (satellite) review.

State:
<paste state.yaml here>

For each holding, report:
- market value, % of NLV, % of Sleeve C
- LOS and Replacement Index score
- 6-month and 12-month relative strength versus the sleeve benchmark
- status: Leader | Neutral | Laggard

Then:
1. Total C as % of NLV vs target and ceiling
2. If above ceiling: which name to trim (the WEAKEST, never the strongest)
3. Laggard list: names in the bottom half for two consecutive quarters.
   For each, state "cut" or "hold" and give ONE line of justification
4. Any name with LOS < 50 or a broken thesis -> mandatory exit
5. Rotation proposal: any candidate that clears the Replacement Index

Rules:
- C is winner-weighted by design. Do NOT propose equal weight
- Never average down a laggard
- C is funded from inside C only, never from A or B
- No rotation on short-term price movement alone
- Confidence % per verdict

Begin.
```

---

## Related

- `../core/sleeves.md` — Sleeve C rules
- `../core/replacement-index.md` — surgery gate
- `../commands/replacement-candidates.md`
