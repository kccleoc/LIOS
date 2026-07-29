# `/portfolio-rotation` — Identify Rotation Opportunities

## Purpose

Identify capital rotation opportunities — moving capital from lower-LFS holdings to higher-LFS holdings to improve overall portfolio construction without necessarily replacing individual holdings.

## When to use

- When new capital is available and Lc wants to know where it should go first
- When evaluating whether to trim a low-LFS holding to fund a higher-LFS opportunity
- Quarterly as part of portfolio optimization
- After a significant portfolio composition change

## Inputs required

1. Live portfolio snapshot
2. Existing LOS and LFS values (generate if unavailable)

## Command

---

```
Analyze the current LIOS portfolio for capital rotation opportunities
using the v1.5 LFS framework.

Current Portfolio Snapshot:
<paste snapshot>

LOS and LFS values:
<paste existing scores or indicate they should be generated>

Output the following sections:

### A. Rotation Priority Matrix

| Priority | Direction | From | To | Rationale | Estimated Impact |
|---|---|---|---|---|---|
| 1 | Add | Capital | TICKER | Highest LFS name under cap | +X% portfolio fit |
| 2 | Rotate | TICKER A | TICKER B | A's LFS < 65, B's LFS > 85, similar LOS | +X% fit, neutral quality |
| 3 | Hold | — | — | No clear rotation opportunity | — |

Directions:
- Add: New capital to a high-LFS name (no exit required)
- Rotate: Move capital from low-LFS to high-LFS (partial or full exit)
- Trim: Reduce low-LFS name to cap without specific replacement
- Hold: No action recommended

### B. Sources of Rotation Capital

Low-LFS holdings that could be reduced:

| Ticker | LOS | LFS | % NLV | LFS Band | Rotate Amount | Destination |
|---|---|---|---|---|---|---|
| ... | ... | ... | ... | Poor Fit / Neutral | $XX (max X%) | TICKER |

### C. Destinations for Rotation Capital

High-LFS names that could absorb more capital:

| Ticker | LOS | LFS | % NLV | Cap Room | Max Add | Priority |
|---|---|---|---|---|---|---|
| ... | ... | ... | ... | X% to cap | $XX | 1 / 2 / 3 |

### D. LFS Improvement Estimate

If the recommended rotations are executed:
- Projected portfolio LFS mean change
- Projected engine diversification change
- Projected quality impact (LOS mean change)

### E. Recommendations

- Top rotation candidate (if any)
- Confidence that rotation would improve portfolio vs status quo
- Any rotation that would violate sleeve caps or other locked decisions

Rules:
- L18 preference rule: when LOS values similar, prefer higher LFS.
- Rotations must not violate sleeve caps (L3 binding).
- A rotation is NOT mandatory — if no clear opportunity exists, state "Hold."
- Rotations that reduce LOS mean by > 3 points require IC approval.

End with: "Rotation Analysis as of YYYY-MM-DD. Top opportunity: [X]. Confidence: XX%."

Begin.
```

---

## What you get back

Rotation priority matrix with sources, destinations, and estimated portfolio impact.

## Discipline reminders

- **A rotation is not a replacement.** Replacement implies a problem with the incumbent. Rotation is opportunistic improvement.
- **Low LFS + High LOS = carry, not crisis.** These are excellent companies that add less portfolio value. Not sell signals.
- **Consider tax implications** — rotation recommendations should note if a sale would trigger taxable gains.

## Related

- See `portfolio-fit.md` — identify which holdings have poor fit
- See `best-diversifiers.md` — find the best diversification candidates
- See `replacement-candidates.md` — evaluate specific replacement scenarios
- See `../lios/components/09_portfolio_construction_intelligence.md` — LFS scale
