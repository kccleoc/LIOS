# `/economic-engine-map` — Map Holdings by Economic Engine

## Purpose

Map every portfolio holding to its underlying economic engine. Reveals true diversification (or concentration) beyond sector labels.

## When to use

- Before any portfolio construction decision
- Quarterly review
- When evaluating whether a new candidate adds genuine diversification
- After adding/removing any position

## Inputs required

1. Live portfolio snapshot

## Command

---

```
Map every holding in the current LIOS portfolio to its primary
economic engine using the v1.5 Economic Engine Principle.

Current Portfolio Snapshot:
<paste snapshot>

Output the following sections:

### A. Economic Engine Map Table

| Economic Engine | Holdings | Total $ Value | % NLV | % of Sleeve B+D | LOS Range | LFS Range |
|---|---|---|---|---|---|---|
| AI Compute | EXAMPLE-B1, ... | $180,000 | 18.0% | 65% | 88-94 | 70-75 |
| AI Memory | EXAMPLE-B2, ... | $100,000 | 10.0% | ... | ... | ... |
| ... | ... | ... | ... | ... | ... | ... |

Economic engines (reference — not exhaustive):
- AI Compute
- AI Memory
- AI Platforms
- Industrial Automation
- Consumer Spending
- Digital Payments
- Healthcare Innovation
- Defense
- Energy Infrastructure
- Enterprise Software
- Logistics
- Utilities

Diversification Principle:
Adding another company driven by the SAME engine → lower incremental LFS.
Adding a company driven by a NEW engine → higher incremental LFS.

### B. Engine Scorecard

| Engine | $ Exposure | % NLV | Diversification Contribution | Risk Level |
|---|---|---|---|---|
| ... | ... | ... | ... | Low / Medium / High / Critical |

Risk levels:
- Low: < 10% NLV
- Medium: 10-25% NLV
- High: 25-40% NLV
- Critical: > 40% NLV

### C. Observations

- Most concentrated engine(s) — what risk does this create?
- Most under-represented engine(s) — which gaps are worth filling?
- Correlation clusters — do multiple engines actually share similar drivers?

### D. Recommendations

- Engines to add (gaps with high-quality candidates available)
- Engines to cap (high concentration — next addition in this engine requires IC approval)
- How this map should influence the next capital allocation decision

End with: "Economic Engine Map as of YYYY-MM-DD. Most concentrated: [engine] at X% NLV. Confidence: XX%."

Begin.
```

---

## What you get back

Full economic engine map with concentration analysis and recommendations.

## Discipline reminders

- **Sector labels can mislead.** Two companies in "Technology" may have entirely different economic engines (AI compute vs enterprise software vs digital payments).
- **This map is the basis for LFS Diversification Value scoring.**
- **Sleeve A (index ETFs)** span multiple engines — note as "diversified" rather than mapping to a single engine.

## Related

- See `portfolio-balance.md` — broader construction assessment
- See `best-diversifiers.md` — find best diversifying candidates
- See `../lios/components/09_portfolio_construction_intelligence.md` — Economic Engine Principle
