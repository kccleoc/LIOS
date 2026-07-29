# `/best-diversifiers` — Identify Best Portfolio Diversifiers

## Purpose

Rank potential investments by their diversification contribution to the current portfolio. Answers: "If I want to make the portfolio more resilient, which candidates should I look at first?"

## When to use

- When Lc wants to reduce concentration in an engine
- When evaluating multiple candidates with similar LOS
- When constructing a shopping list for SR deployment
- Before any portfolio rotation

## Inputs required

1. Live portfolio snapshot
2. List of candidate tickers or company names (or "auto-scan" to search LIOS watchlist)
3. Existing LOS values (generate if unavailable)

## Command

---

```
Identify the best portfolio diversifiers from the given candidate
list, using the LIOS v1.5 LFS framework.

Current Portfolio Snapshot:
<paste snapshot>

Candidates: <TICKER1, TICKER2, ... or "AUTO: scan the LOS watchlist">

If LOS values per candidate exist, use them. Otherwise generate them.

Output the following:

### A. Diversifier Ranking

| Rank | Ticker | LOS | LFS | Economic Engine | Diversification Value | Overlap Score | Fit Verdict |
|---|---|---|---:|---:|---|---:|---:|
| 1 | ... | ... | ... | New engine? | XX | XX | Exceptional / Strong / Useful / Poor |

Ranked by LFS Diversification Value component (weighted with LOS).

### B. Engine Gap Fillers

For each candidate that introduces a NEW economic engine:

| Candidate | New Engine | LOS | LFS | Priority |
|---|---|---|---|---|
| ... | ... | ... | ... | High / Medium / Low |

### C. Non-Diversifiers (Overlap Alert)

For each candidate that overlaps heavily with existing engines:

| Candidate | Overlapping Engine | Existing Holdings | Overlap Score | Recommendation |
|---|---|---|---|---|
| ... | ... | ... | XX | Lower priority / Pass unless exceptional LOS |

### D. Recommendations

- Top 3 diversifiers by LFS — where they would fit, how much to allocate
- Any diversifier with LOS < 70 — note but do not recommend (L17)
- Priority ordering for capital allocation (L18 preference rule applied)

Rules:
- Sleeve A vehicles excluded.
- A candidate that adds NO new economic engine AND scores Overlap < 50 is automatically low priority.
- L17 applies: high LFS does not rescue low LOS.

Begin.
```

---

## What you get back

Ranked diversifier list showing which candidates add genuine portfolio diversification.

## Discipline reminders

- **Diversification is not a substitute for quality.** A low-LOS diversifier is still a pass.
- **Sleeve A already provides broad diversification** — this analysis is for Sleeve B + D candidates.
- **Same-engine candidates can still be buys** — but they need higher LOS to compensate.

## Related

- See `economic-engine-map.md` — understand current engine exposure
- See `portfolio-rotation.md` — rotate from low to high LFS
- See `../lios/components/09_portfolio_construction_intelligence.md` — LFS Diversification Value component
