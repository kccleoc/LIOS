# `/replacement-candidates` — Find Best Replacement Candidates

## Purpose

When considering whether to replace an existing holding, scan candidates and rank them by LOS + LFS + Replacement Index compatibility. Answers: "If I replace X, who is the best alternative?"

## When to use

- When a Sleeve D holding is underperforming or LOS is deteriorating
- When a Sleeve B holding is near cap and needs trimming consideration
- Before quarterly Sleeve D review
- When Lc asks "what else could I own instead of X?"

## Inputs required

1. Live portfolio snapshot
2. Incumbent ticker being evaluated for replacement
3. Candidate list (or "auto-scan" the LOS watchlist)
4. Replacement Index scores for incumbent (generate if unavailable)

## Command

---

```
Find the best replacement candidates for the specified incumbent
holding, using the LIOS v1.5 framework.

Current Portfolio Snapshot:
<paste snapshot>

Incumbent: <TICKER>
- Sleeve: <B or D>
- Current LOS: <NN>
- Current LFS: <NN>
- Current Replacement Index (if Sleeve D): <NN>
- Reason for replacement evaluation: <reason>

Candidates: <TICKER1, TICKER2, ... or "AUTO: scan LOS watchlist">

If any scores are missing, generate them.

Output the following:

### A. Replacement Candidate Ranking

| Rank | Candidate | LOS | LFS | RI (if D) | Economic Engine | LFS vs Incumbent | Recommendation |
|---|---|---|---|---|---|---|---|
| 1 | ... | ... | ... | ... | ... | Higher / Lower | Replace / Watch / Pass |

### B. Prerequisite Checks

For each candidate:

1. LOS gate check: LOS ≥ 70? (If no, reject — L13)
2. LOS quality check: LOS ≥ 75 for Sleeve D? (If no, requires IC override)
3. LFS check: Does candidate improve portfolio fit vs incumbent?
4. Engine check: Does candidate add or maintain engine diversification?

### C. Joint Evaluation (Sleeve D)

For Sleeve D incumbents, perform the full joint workflow:

1. Candidate LOS gate (≥ 75 typically required)
2. Replacement Index: Candidate RI > Incumbent RI?
3. Qualitative check: ≥ 2 of 4 conditions met? (earnings miss, thesis weakens, valuation, wave conviction)
4. LFS comparison: Candidate LFS vs Incumbent LFS
5. Overall verdict

### D. Recommendations

- Best replacement candidate with rationale
- Any candidate that should be added WITHOUT replacing (if under sleeve cap)
- Any candidate that fails the LOS gate — do not advance

Rules:
- Sleeve D replacement requires BOTH LOS gate AND Replacement Index superiority.
- Sleeve B replacement is rare — caps are binding, replacement is separate from cap management.
- L17 applies: high LFS alone cannot qualify a low-LOS candidate for replacement.
- If no candidate is clearly superior, recommend "Hold — re-evaluate next quarter."

Begin.
```

---

## What you get back

Ranked replacement candidates with full LOS + LFS + Replacement Index evaluation.

## Discipline reminders

- **Replacement is not rotation.** Replacement requires a specific problem with the incumbent. Rotation is opportunistic improvement.
- **Sleeve B caps are not replacement triggers.** A cap is a buying-pause, not an exit signal.
- **If the best candidate has lower LFS than the incumbent**, replacing would reduce portfolio fit — flag this explicitly.

## Related

- See `los-replace.md` — v1.4 replacement command (uses LOS + Replacement Index)
- See `portfolio-rotation.md` — rotation analysis (for improving overall fit)
- See `../lios/components/04_replacement_index.md` — Replacement Index spec
- See `../lios/components/09_portfolio_construction_intelligence.md` — LFS spec
