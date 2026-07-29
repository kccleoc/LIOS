# `/los-sleeve-d` — Sleeve D LOS + Frontier Review

## Purpose

Review Sleeve D holdings using BOTH the Research Intelligence Layer
(LOS) AND the Replacement Index (Frontier Score / Thesis Momentum /
Diversification Value). This is the joint evaluation that gates
Sleeve D surgery.

## When to use

- Quarterly Sleeve D review (default cadence)
- After any D holding earnings or thesis event
- When considering a Sleeve D replacement

## Inputs required

1. Current Sleeve D holdings (snapshot rows)
2. Current LOS per holding
3. Optional: any candidate names being considered

## Command

---

```
Review Sleeve D holdings using both the LOS framework and the
Replacement Index (LIOS joint workflow).

Current Sleeve D holdings:
<paste holdings>

Candidates to evaluate (optional):
<list candidates or "none — review incumbents only">

For each name, score:
- LOS components (LVS, LGS, LQS, LMS) with one-line rationale
- Replacement Index components (Frontier, Momentum, Diversification)
  with one-line rationale

Output a unified table:

| Ticker | Stage | LOS | Band | Frontier (40%) | Momentum (35%) | Diversification (25%) | Replacement Index | Verdict |
|--------|-------|----:|------|---------------:|---------------:|---------------------:|------------------:|---------|
| ... | ... | ... | ... | ... | ... | ... | ... | ... |

Then a verdict block per holding:
- Hold: no surgery warranted
- Hold + monitor: LOS or Frontier approaching threshold
- Replace: candidate clears both LOS (≥75) and Replacement Index
  (composite superior + ≥2 of 4 qualitative conditions)

Rules:
- LOS < 70 → automatic Pass; do NOT advance even if Frontier is high
- LOS 70-74 → flag as Watch; review next cycle
- LOS ≥ 75 → eligible for Sleeve D consideration, but Replacement
  Index still gates all surgery
- Replacement Index approval rules unchanged (L5)
- Sleeve D staging (Stage 1 → 2 → 3) still applies to new names

Begin.
```

---

## What you get back

Joint LOS + Replacement Index scoreboard per D holding, with
replacement verdicts.

## Example output

| Ticker | Stage | LOS | Band | Frontier | Momentum | Diversification | RI Composite | Verdict |
|---|---:|---:|---|---:|---:|---:|---:|---|
| EXAMPLE-D1 | 3 | 82 | Buy | 75 | 65 | 55 | 66.5 | Hold |
| EXAMPLE-D2 | 3 | 88 | Buy | 70 | 75 | 60 | 69.3 | Hold |
| EXAMPLE-D3 | 2 | 74 | Watch | 60 | 55 | 45 | 54.0 | **Hold + monitor** (LOS near threshold; still in pilot) |
| <candidate> | n/a | 86 | Buy | 78 | 72 | 65 | 72.5 | Eligible for surgery if RI conditions met |

**Verdicts**: No surgery warranted this cycle. EXAMPLE-D3 LOS at 74 — re-evaluate next quarter. No candidates yet cleared RI superiority + 2 of 4 conditions.

## Discipline reminders

- **LOS < 70 in Sleeve D = Pass.** Don't paper over with Frontier narrative.
- **LOS alone does not trigger surgery.** Replacement Index gates.
- **Staging still applies.** SR-funded D expansion does NOT bypass Stage 2/3.

## Related

- See `../lios/components/04_replacement_index.md` — RI framework
- See `../lios/components/08_research_intelligence_layer.md` — LOS framework
- See `los-replace.md` — replacement decision workflow
- See `sleeve-d-review.md` — RI-only workflow (pre-v1.4)