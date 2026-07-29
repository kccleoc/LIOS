# `/kelly-recalc` — Recompute Kelly Factor

## Purpose

Recompute the Kelly Factor for the current DCA cycle. This is the
fastest single workflow in LIOS — runs in minutes, no full report needed.

## When to use

- **Always** before executing monthly DCA
- When macro or valuation conditions shift materially
- After Sleeve B cap hits (changes Portfolio Valuation dimension)
- After Sleeve D surgery (changes Opportunity Set dimension)

## Inputs required

1. Live portfolio snapshot
2. Current macro data points (or note that AI will use most recent)
3. Any specific concerns Lc wants surfaced

## Command

---

```
Recompute the LIOS Kelly Factor for this DCA cycle.

Snapshot:
<paste live snapshot here>

Score the five Kelly v2.0 dimensions on 0-100 scale:
1. Market Valuation (30%)
2. Portfolio Valuation (25%)
3. Macro & Liquidity (20%)
4. Technical Conditions (15%)
5. Opportunity Set (10%)

For each dimension: score + one-line rationale citing the
specific data point driving it.

Output:
- Weighted total
- Kelly Factor (0.00 / 0.10 / 0.25 / 0.50 / 0.75 / 1.00)
- Dollar amount to deploy this month
- Dollar amount going to Kelly Reserve
- Any single-dimension concerns worth flagging

Confidence: XX% on the Kelly Factor itself.

Begin.
```

---

## What you get back

A compact table + recommendation:

| Dimension | Score | Rationale |
|---|---|---|
| Market Valuation (30%) | XX | <one line> |
| Portfolio Valuation (25%) | XX | <one line> |
| Macro & Liquidity (20%) | XX | <one line> |
| Technical Conditions (15%) | XX | <one line> |
| Opportunity Set (10%) | XX | <one line> |
| **Weighted Total** | **XX.X** | |
| **Kelly Factor** | **0.XX** | |
| Deploy this month | $X,XXX | |
| To Kelly Reserve | $XXX | |

Confidence: XX%

## Related

- See `../lios/components/03_kelly_model_v2.md` — full framework spec
- See `commands/health-check.md` §3 — Kelly Dashboard as part of full Health Check