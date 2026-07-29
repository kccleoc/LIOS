# `/portfolio-balance` — Portfolio Construction Assessment

## Purpose

Assess the portfolio's overall construction quality — economic engine diversification, concentration risks, sleeve balance, and structural gaps. This is the portfolio-level complement to the holding-level LFS analysis.

## When to use

- Quarterly as part of Health Check
- Before any sleeve rebalancing
- After adding or exiting significant positions
- When Lc asks "is my portfolio well-constructed?"

## Inputs required

1. Live portfolio snapshot
2. Existing LOS and LFS values per holding (generate if unavailable)

## Command

---

```
Generate a Portfolio Construction Assessment for the current LIOS
portfolio using the v1.5 framework.

Current Portfolio Snapshot:
<paste snapshot>

LOS and LFS:
<paste existing scores or indicate they should be generated>

Output the following sections:

### A. Economic Engine Map

| Economic Engine | Holdings | Total $ | % NLV | Concentration Rating |
|---|---|---|---|---|
| AI Compute | EXAMPLE-B1 ... | $X | X% | High / Medium / Low |
| AI Memory | EXAMPLE-B2 ... | $X | X% | ... |
| ... | ... | ... | ... | ... |

Concentration rating:
- < 10% NLV per engine: Low (well-diversified)
- 10-25%: Medium (monitor)
- > 25%: High (flag as concentration risk)
- > 40%: Critical (requires IC attention)

### B. Engine Gap Analysis

Economic engines with ZERO exposure that could fit the LIOS mandate.
Rate each as: Strong candidate / Weak candidate / Not applicable.

### C. Overlap Clusters

Groups of holdings sharing the same economic engine.
For each cluster: holdings, total exposure, risk assessment.

### D. Sleeve Construction

- % NLV in each sleeve vs target
- Sleeve-level LFS mean (B and D only)
- Any sleeve where the lowest-LFS holding should be evaluated

### E. Construction Score

Score the portfolio on each dimension (0-100):

| Dimension | Score | Assessment |
|---|---|---|
| Engine Diversification | XX | ... |
| Sleeve Balance | XX | ... |
| Concentration Risk | XX | ... |
| Overall Construction | XX | ... |

### F. Recommendations

- Engines to add exposure to (gaps)
- Engines to reduce exposure in (over-concentrated)
- Specific candidate types that would improve portfolio construction

End with: "Portfolio Construction Score: XX/100. Priority: [add engine X / reduce engine Y / maintain]. Confidence: XX%."

Begin.
```

---

## What you get back

Full portfolio construction assessment with economic engine map, gap analysis, and construction score.

## Discipline reminders

- **Economic Engine Principle** — diversification is about underlying economic drivers, not sector labels.
- **An over-concentrated engine is a risk even if individual holdings are high-LOS.**
- **A gap is not a mandate to fill** — if no suitable high-LOS candidates exist in an engine, hold.

## Related

- See `economic-engine-map.md` — detailed engine mapping
- See `best-diversifiers.md` — best diversification candidates
- See `portfolio-fit.md` — holding-level fit analysis
- See `../lios/components/09_portfolio_construction_intelligence.md` — Economic Engine Principle
