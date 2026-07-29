# `/portfolio-los` — Full Portfolio LOS Dashboard

## Purpose

Generate the complete LOS profile across the entire portfolio in one
pass. The canonical output that goes into the Health Check (§5, §6,
§8, §11) and into the snapshot file for archival.

## When to use

- After any major LOS-affecting event (earnings, guidance, M&A)
- Before a full Health Check
- Monthly / quarterly review
- When Lc wants "what does my portfolio look like through LOS?" answered

## Inputs required

1. Live portfolio snapshot
2. Optional: any LOS values Lc has already computed (to seed / verify)

## Command

---

```
Generate the full LIOS portfolio LOS dashboard.

Snapshot:
<paste live snapshot>

If current LOS values per holding exist, include them; otherwise
generate them via /los for each holding.

Output sections:

### A. Sleeve B LOS (Health Check §6)

| Ticker | Shares | MV | % NLV | LOS | Band | LVS | LGS | LQS | LMS | Rec |
| ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |

### B. Sleeve D LOS + Replacement Index (Health Check §8 / §11)

| Ticker | Shares | MV | % NLV | Stage | LOS | Frontier | Momentum | Diversification | RI | Rec |
| ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... |

### C. Portfolio Summary

- Highest LOS holding(s)
- Lowest LOS holding(s)
- Mean / median LOS (Sleeve B + D only — Sleeve A excluded)
- Distribution by band:
    - Elite (90-100): N holdings, X% NLV
    - Buy (80-89):    N holdings, X% NLV
    - Watch (70-79):  N holdings, X% NLV
    - Hold (60-69):   N holdings, X% NLV
    - Pass (<60):     N holdings, X% NLV

### D. Recommendations

- Any holding LOS < 70 → flag as Pass; existing holders monitor;
  evaluate via /los-replace if in Sleeve D
- Any holding LOS ≥ 90 → confirm conviction; document why held at
  current size (vs cap / vs available capital)
- Cross-sleeve observations (e.g., "Sleeve B concentrated in Elite
  names; Sleeve D has one Watch-band holding approaching Pass")

### E. Date-stamped output

End with: "Portfolio LOS as of YYYY-MM-DD. NLV: $X. Sleeve B+D
LOS mean: XX. Confidence on overall assessment: XX%."

Rules:
- Sleeve A vehicles (from `state.yaml: positions` filtered to sleeve: A) excluded — LOS not applicable
- All component scores must have a one-line rationale
- Default weighting: LQS 35 / LGS 25 / LVS 25 / LMS 15; disclose deviation
- Confidence % required on overall assessment

Begin.
```

---

## What you get back

Full portfolio LOS dashboard — copy/paste-ready for Health Check or snapshot archival.

## Example output (excerpt)

### C. Portfolio Summary

- **Highest LOS**: EXAMPLE-B1 at 94 (Elite)
- **Lowest LOS**: EXAMPLE-D3 at 74 (Watch — near threshold)
- **Mean LOS** (B+D): 84.3
- **Distribution:**
    - Elite: 2 holdings (EXAMPLE-B1, EXAMPLE-B3), 30.0% NLV
    - Buy: 3 holdings (EXAMPLE-B2, EXAMPLE-D1, EXAMPLE-D2), 14.4% NLV
    - Watch: 1 holding (EXAMPLE-D3), 1.05% NLV

### D. Recommendations

- **EXAMPLE-D3** at LOS 74 — Watch band, near Pass threshold. Re-evaluate next cycle; if drops below 70, evaluate replacement via `/los-replace`.
- **EXAMPLE-B1** at LOS 94, near Sleeve B cap. Conviction intact; no add until trim or cap revision (which is an L3 form amendment, requiring a major bump — the cap values in `state.yaml: sleeve_b_caps` are portfolio-local parameters).

### E. Date-stamped

Portfolio LOS as of 2026-07-25. NLV: $202,012. Sleeve B+D LOS mean: 84.3. Confidence: 80%.

## Discipline reminders

- **Re-score after every earnings** for Sleeve B and key D holdings.
- **Aging LOS values** (90+ days without re-score) should be flagged for refresh.
- **Snapshot archival**: paste the dashboard into the snapshot file under a "Portfolio LOS" section.

## Related

- See `los-top.md` — quick top-by-LOS
- See `los-sleeve-b.md` — Sleeve B only
- See `los-sleeve-d.md` — Sleeve D only
- See `commands/health-check.md` — full report uses this output