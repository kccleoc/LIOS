> Quick reference — full methodology at core/regime-index.md

# Bull–Bear Index — Quick Reference

0–100. **Higher = more bearish / more hostile to deployment.**

## Five Dimensions

| Dimension | Weight |
|---|---:|
| Market Valuation | 30% |
| Portfolio Valuation | 25% |
| Macro & Liquidity | 20% |
| Technical Conditions | 15% |
| Opportunity Set | 10% |

```
BBI = 0.30·MV + 0.25·PV + 0.20·ML + 0.15·T + 0.10·O
```

## Bands

| BBI | Band | A | B | C | D | Z |
|---|---|---:|---:|---:|---:|---:|
| 0–35 | Risk-On | 65% | 9% | 6% | 15% | 5% |
| 36–50 | Constructive | 66% | 8.5% | 5.5% | 15% | 5% |
| 51–65 | Cautious | 70% | 6% | 4% | 15% | 5% |
| 66–100 | Defensive | 73% | 4% | 3% | 15% | 5% |

Total risky exposure is **80% in every band**. The index shifts only the split
between passive index and active bets.

## Three Rules

1. Never changes total risky exposure.
2. Implemented by **starving** B and C of contributions — never by selling.
3. One band step per quarter.

Core: `core/regime-index.md` | Command: `commands/regime-recalc.md`
