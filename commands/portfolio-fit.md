# `/portfolio-fit` — Full Portfolio Fit Analysis

## Purpose

Analyze the entire portfolio through the LFS lens. Score every holding on portfolio fit, identify which names are earning their keep and which are merely being carried.

## When to use

- After any portfolio composition change
- Quarterly as part of the full Health Check
- When Lc asks "does this portfolio make sense as a system?"
- Before major rebalancing or rotation decisions

## Inputs required

1. Live portfolio snapshot
2. Existing LOS values per holding (generate if unavailable)

## Command

---

```
Generate the full LIOS v1.5 Portfolio Fit Analysis.

Current Portfolio Snapshot:
<paste snapshot>

If LOS values per holding exist, include them. Otherwise generate them.

Output the following sections:

### A. Portfolio Fit Dashboard

| Ticker | Sleeve | % NLV | LOS | LFS | Decision Matrix | Fit Trend |
|---|---|---|---|---|---|---|
| ... | ... | ... | ... | ... | ... | ... |

Decision Matrix key:
- High LOS + High LFS = "Core" — optimal holding
- High LOS + Low LFS = "Carry" — good company, low portfolio contribution
- Low LOS + High LFS = "Diversifier" — needs LOS improvement to be core
- Low LOS + Low LFS = "Candidate" — should be evaluated for replacement

Fit Trend: Improving / Stable / Deteriorating (based on recent changes)

### B. Portfolio Fit Summary

- Holdings by Decision Matrix category (Core / Carry / Diversifier / Candidate)
- % NLV in each category
- Mean LFS (Sleeve B + D)
- LFS distribution by band

### C. Key Observations

- Best-fit holdings and why
- Worst-fit holdings and why
- Any holdings where LFS is notably lower than LOS (excellent companies with poor portfolio fit)
- Any holdings where LFS is notably higher than LOS (diversification stars with moderate quality)

### D. Recommendations

- Candidates for rotation (low LFS names that could be replaced)
- Candidates for additional capital (high LFS names with room under cap)
- Priority ordering for new capital based on LFS (L18 preference rule)

End with: "Portfolio Fit as of YYYY-MM-DD. Portfolio LFS mean: XX. Confidence: XX%."

Rules:
- Sleeve A vehicles (from `state.yaml: positions` filtered to sleeve: A) excluded — LFS not applicable to index vehicles
- LFS is dynamic — flag if any score is >90 days old
- L18 preference rule: when LOS values are similar, higher LFS takes priority

Begin.
```

---

## What you get back

Full portfolio fit dashboard with decision matrix across all holdings.

## Discipline reminders

- **LFS is portfolio-relative** — same holding scored in a different portfolio would have different LFS.
- **"Carry" holdings are not sell signals** — they are lower priority, not replacement candidates.
- **"Diversifier" holdings (Low LOS + High LFS) are not buys** — L17 rule applies.

## Related

- See `lfs.md` — single-company LFS scoring
- See `portfolio-balance.md` — portfolio construction assessment
- See `portfolio-rotation.md` — rotation opportunity analysis
- See `../lios/components/09_portfolio_construction_intelligence.md` — full LFS spec
