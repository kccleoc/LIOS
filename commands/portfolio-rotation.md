# `/portfolio-rotation` — Identify Rotation Opportunities

## Purpose

Identify where capital may legitimately move under v5. There are exactly two
rotation seams, and neither is a rotation book:

| Seam | Operation | Bounded by |
|---|---|---|
| **Sleeve B** — replace the weakest satellite | Replacement, **size-neutral**: one-out-one-in at the same dollars | Replacement Index + B ceiling |
| **Sleeve A** — change a thematic view | View rotation, under a Transition Mandate with a sunset date | P3a, P6′ caps |

**Sleeve C is not a rotation book.** C holds bonds. It is bought to target, never
ranked, never winner-weighted, and has no Replacement Index. "Rotate the weakest
in C" is a deleted v4.1 rule.

New capital is not a rotation question either. It is the monthly Top-up, which is
mechanical (P3′).

## When to use

- Quarterly, as the Sleeve B replacement review
- When a B holding's Replacement Index has fallen behind a qualifying candidate
- When an A thematic view is being retired and another put in its place
- When Lc asks "what else could I own instead of X?"

## Inputs required

1. Live portfolio snapshot
2. Existing LOS, LFS and Replacement Index values (generate if unavailable)
3. For an A thematic change: the proposed view statement, horizon, and
   invalidation condition

## Command

---

```
Analyze the current LIOS v5 portfolio for legitimate rotation opportunities.

Current Portfolio Snapshot:
<paste snapshot>

LOS, LFS and Replacement Index values:
<paste existing scores or indicate they should be generated>

Two seams exist. Evaluate both, then stop.

### A. Rotation Priority Matrix

| Priority | Direction | From | To | Rule cited | Estimated Impact |
|---|---|---|---|---|---|
| 1 | Replace | Sleeve B incumbent | Sleeve B candidate | Replacement Index + IC | Size-neutral |
| 2 | View rotation | Sleeve A thematic ETF | Sleeve A thematic ETF | Transition Mandate (P3a) | Risky weight unchanged |
| 3 | Hold | — | — | No qualifying candidate | — |

Directions:
- Replace: Sleeve B only. One-out-one-in at the same dollar value. A replacement
  is not a partial exit and does not change risky weight
- View rotation: Sleeve A only, inside a Transition Mandate with a sunset date and
  completion criteria in dollars
- Hold: no action. This is a legitimate answer

There is no "rotate from low LFS to high LFS" direction. Low LFS with high LOS is
carry, not a sell signal, and a holding is never sold because capital is wanted
elsewhere.

### B. Sleeve B Replacement Sources

| Ticker | LOS | LFS | RI | % NLV | Status | Replace Amount | Destination |
|---|---|---|---|---|---|---|---|
| ... | ... | ... | ... | ... | Weakest / Mid / Strongest | size-neutral | <TICKER> |

Rank every B name by Replacement Index. Name the weakest. The weakest is the
standing replacement candidate, subject to the ceiling.

### C. Sleeve A View-Rotation Candidates

| Holding | Role (core / thematic) | View statement current? | Invalidation met? | Proposed replacement | Mandate required |
|---|---|---|---|---|---|
| ... | ... | Yes / No | Yes / No | ... | Yes (P3a) |

A thematic ETF is rotated on view, not on price. A constituent company's failure
does not invalidate the basket view.

### D. Impact Estimate

If the recommended moves execute:
- Replacement Index mean change across Sleeve B
- Post-trade B weight vs ceiling, and post-trade risky weight = (A + B) / NLV
- Look-through single-company and sector exposure after the swap, against P6' caps
- Realised gain or loss, and whether any sale is taxable

### E. Recommendations

- Top candidate with rationale
- Confidence that the move improves the portfolio versus status quo
- Any move that would breach a cap or the P4' direction limits

Rules:
- Sleeve B replacement requires BOTH Replacement Index composite superiority AND
  at least 2 of the 4 qualitative conditions
- Replacement is size-neutral. One out, one in, same dollars. No net addition
- Sleeve B is never funded from Top-up, and never from C
- Sleeve A to Sleeve B is an Investment Committee decision, limited by B ceiling
  headroom. It is never an automatic output of this analysis
- An A thematic rotation needs a Transition Mandate: sunset date, completion
  criteria in dollars, and cap compliance throughout
- C appear here only as Rebalance sources when a risky sleeve is underweight
  and a band is breached. They are not rotation destinations
- A recommended sale that is neither a breach cure, a thesis break, nor a mandate
  action is deleted from the plan
- Rotations that would reduce mean LOS by more than 3 points require IC approval

End with: "Rotation Analysis as of YYYY-MM-DD. Top opportunity: [X]. Confidence: XX%."

Begin.
```

---

## What you get back

A two-seam matrix: Sleeve B replacements, Sleeve A view rotations, and an impact
estimate in dollars. Anything that is not one of those two is reported as Hold.

## Discipline reminders

- **A replacement is not a rotation.** Replacement requires a specific problem with
  the incumbent, proven by the Replacement Index. Rotation-on-opportunity is gone.
- **Replacement is size-neutral.** Same dollars in as out. It never raises B.
- **Low LFS + high LOS = carry, not crisis.** Excellent companies that add less
  portfolio value. Not sell signals.
- **C is not rotated.** Bonds are bought to target and sold pro-rata.
- **Consider tax implications** — note when a sale would trigger taxable gains.

## Related

- See `portfolio-fit.md` — identify which holdings have poor fit
- See `best-diversifiers.md` — find the best diversification candidates
- See `replacement-candidates.md` — Replacement Index screen for a specific incumbent
- See `../core/replacement-index.md` — the replacement test
- See `../core/deployment.md` — Top-up, Rebalance, Transition Mandate
