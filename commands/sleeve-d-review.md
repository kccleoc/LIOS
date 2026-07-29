# `/sleeve-d-review` — Replacement Index Workflow

## Purpose

Run the Replacement Index on every current Sleeve D holding, plus
any candidate names being considered. Surfaces which (if any)
surgery is justified.

## When to use

- **Quarterly** as a default Sleeve D review
- When Lc has a candidate replacement in mind
- After major earnings from a D holding
- After a thesis event (breakthrough, regulation, M&A)

## Inputs required

1. Live portfolio snapshot
2. List of current D holdings + any candidate names being considered
3. Optional: any specific events Lc wants surfaced (earnings, guidance, etc.)

## Command

---

```
Run a LIOS Sleeve D Review using the Replacement Index framework.

Current Sleeve D holdings:
<list holdings here>

Candidates to evaluate (if any):
<list candidates here, or "none — review incumbents only">

For each name (incumbent + candidate), score three metrics 0-100:

1. Frontier Score (40%) — positioning within frontier tech theme
2. Thesis Momentum (35%) — is the thesis strengthening or weakening?
3. Diversification Value (25%) — value-add vs other D holdings

For each score: one-line rationale citing the specific evidence.

Output:
- Per-name metric table
- Composite Replacement Index per name
- For each incumbent: composite vs highest-scoring candidate
- Replacement verdict per incumbent:
  - "Hold" — no candidate clears the bar
  - "Hold + monitor" — close but not quite
  - "Replace" — candidate clears bar (state which + why)
- Compliance check: any proposed surgery respects funding sources
  (D funded by external capital only — never DCA / A sales / Kelly)
- Compliance check: any new name would still need to clear
  Stage 1 / 2 / 3 staging rules

Rules:
- Replacement requires Composite > AND ≥2 of 4 qualitative conditions
- No surgery on short-term price movement alone
- Confidence % per verdict
- If no surgery warranted, say so explicitly — "Hold all D. No action."

Begin.
```

---

## What you get back

A Sleeve D scoreboard + surgery recommendations.

## Example output shape

| Name | Frontier (40%) | Momentum (35%) | Diversification (25%) | Composite |
|---|---|---|---|---|
| EXAMPLE-D1 | 75 | 65 | 55 | 66.5 |
| EXAMPLE-D2 | 70 | 75 | 60 | 69.25 |
| <candidate X> | 80 | 70 | 65 | 73.0 |

Verdicts:
- **EXAMPLE-D1**: Hold. No candidate clearly superior.
- **EXAMPLE-D2**: Hold. Thesis intact, no replacement candidate clears bar.
- **`<EXITED-NAME>` → <candidate X>**: Replace. Composite 73 > 47.5. Two of four conditions met: missed earnings + cheaper valuation. Confidence: 80%.

## Discipline reminders

- **Scores are dynamic**, not permanent. Re-run quarterly or on thesis events.
- **Stage rules still apply** even after Replacement Index approval. SR-funded D expansion does not bypass Stage 2/3 gates.
- **No replacement on short-term price action** alone. Use the framework.

## Related

- See `../lios/components/04_replacement_index.md` — full framework spec
- See `../lios/components/06_health_check_structure.md` §8, §11 — D Review and Frontier Scoreboard placement in Health Check
- See `../lios/components/02_capital_architecture.md` Sleeve D — staging and funding rules