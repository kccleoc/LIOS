# `/replacement-candidates` — Find Best Replacement Candidates

## Purpose

When considering whether to replace an existing holding, scan candidates and rank them by LOS + Replacement Index compatibility. Answers: "If I replace X, who is the best alternative?"

**The Replacement Index governs Sleeve B.** It applies to nothing else. Sleeve A
holds ETFs and is rotated on view, not ranked. Sleeve C holds bonds, is bought to
target, and is never ranked against itself. Sleeve D does not exist.

## When to use

- When a Sleeve B holding's thesis is weakening or its LOS is deteriorating
- Before the quarterly Sleeve B replacement review
- When Sleeve B is at its ceiling and a candidate would displace an incumbent
- When Lc asks "what else could I own instead of X?"

## Inputs required

1. Live portfolio snapshot
2. Incumbent ticker being evaluated for replacement (Sleeve B only)
3. Candidate list (or "auto-scan" the LOS watchlist)
4. Replacement Index scores for the incumbent (generate if unavailable)

## Command

---

```
Find the best replacement candidates for the specified incumbent
holding, using LIOS v6.

Current Portfolio Snapshot:
<paste snapshot>

Incumbent: <TICKER>  (must be a Sleeve B holding)
- Current LOS: <NN>
- Current LFS: <NN>
- Current Replacement Index: <NN>
- Current weight: <NN% of NLV>, <dollars>
- Reason for replacement evaluation: <reason>

Candidates: <TICKER1, TICKER2, ... or "AUTO: scan LOS watchlist">

If any scores are missing, generate them.

Output the following:

### A. Replacement Candidate Ranking

| Rank | Candidate | LOS | LFS | RI | RI vs Incumbent | Engine | Recommendation |
|---|---|---|---|---|---|---|---|
| 1 | ... | ... | ... | ... | +NN.N | New / Overlap | Replace / Watch / Pass |

Composite: Replacement_Index = 0.40*Frontier + 0.35*Momentum + 0.25*Diversification

### B. Prerequisite Checks

For each candidate:

1. LOS gate: LOS >= 80 for a new Sleeve B name. LOS < 70 is an automatic Pass
2. Replacement Index: candidate composite strictly greater than incumbent
3. Qualitative conditions: at least 2 of 4
   - incumbent missed earnings or issued weak guidance
   - incumbent thesis weakened materially
   - candidate materially cheaper on forward valuation
   - candidate's wave conviction materially higher
4. Portfolio fit: does the candidate improve or maintain LFS?
5. Engine check: does the candidate add or maintain economic-engine diversification?

### C. Sizing and Ceiling Check

1. Replacement is size-neutral: one out, one in, at the SAME dollar value
2. Post-trade Sleeve B weight vs the ceiling (`state.yaml: sleeves.b.ceiling_pct`),
   in dollars and percent of NLV
3. Post-trade look-through single-company and sector exposure vs the P6' caps
4. IC approval required before the trade. State it as a required step, not a
   formality

### D. Recommendations

- Best candidate with rationale and Confidence %
- Any candidate worth adding WITHOUT replacing — only if Sleeve B is below its
  ceiling AND Sleeve A funds it as an IC decision. New capital from Top-up is not
  available to Sleeve B, ever
- Any candidate that fails the LOS gate — do not advance

Rules:
- Sleeve B replacement requires BOTH the LOS gate AND Replacement Index superiority,
  plus 2 of 4 qualitative conditions. Two out of three is not enough
- Replacement never adds dollars to Sleeve B. If the result would raise B, it is
  not a replacement — it is an A-to-B funding decision and needs an IC
- A candidate's price rising is not a reason to replace, and the incumbent's
  unrealised loss is not a reason either. Sunk cost is not a metric
- Sleeve B caps are not replacement triggers. At the ceiling, trim the largest
  position at the quarterly review; replacement is a separate judgment
- If no candidate is clearly superior, recommend "Hold — re-evaluate next quarter."

Begin.
```

---

## What you get back

Ranked replacement candidates with full LOS + Replacement Index evaluation, a
size-neutral sizing check, and a ceiling and look-through compliance line.

## Discipline reminders

- **The Replacement Index is Sleeve B's instrument.** Misapplied to A or C it
  produces rotation advice that v6 prohibits.
- **Replacement requires a problem with the incumbent** — proven by composite
  superiority plus 2 of 4 conditions. Opportunity alone is not a reason.
- **Sleeve B caps are not replacement triggers.** A cap is a buying pause, not an
  exit signal.
- **If the best candidate has lower LFS than the incumbent**, replacing reduces
  portfolio fit — flag it explicitly.

## Related

- See `los-replace.md` — LOS + Replacement Index workflow
- See `portfolio-rotation.md` — the two legitimate rotation seams
- See `../core/replacement-index.md` — Replacement Index specification
- See `../core/sleeves.md` — Sleeve B entry, ceiling and trim rules
