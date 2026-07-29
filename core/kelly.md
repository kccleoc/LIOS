> **Source:** Extracted verbatim from LIOS v1.5. L4 — Kelly Model v2.0 is a Locked Decision.

# Kelly Model v2.0 — Five-Dimension Scoring

**Component version:** v1.3
**Status:** Locked Decision (framework) / Dynamic (scores)
**Last updated:** 2026-07-25

---

## Purpose

Kelly Model v2.0 produces a single 0-1 fraction — the **Kelly Factor** —
that governs what portion of the monthly DCA (`state.yaml: monthly_dca`; $1,500 default) gets deployed.
The undeployed portion sits in Sleeve C as Kelly Reserve until
either Kelly increases or a separate trigger fires.

The model converts five orthogonal market/portfolio inputs into one
deployable number. It is not a market-timing tool in the discretionary
sense — it is a structured overlay.

---

## Five Dimensions

Each dimension is scored 0-100 by the AI based on current data.
Higher score = worse conditions for deployment (more conservative).
Lower score = better conditions (more aggressive).

| # | Dimension | Weight | What it measures |
|---|---|---|---|
| 1 | **Market Valuation** | 30% | Broad equity valuations — P/E, CAPE, equity risk premium, breadth |
| 2 | **Portfolio Valuation** | 25% | Valuation of Lc's specific holdings — P/E vs own history, growth durability |
| 3 | **Macro & Liquidity** | 20% | Rates, credit spreads, liquidity conditions, Fed posture, USD |
| 4 | **Technical Conditions** | 15% | Trend, momentum, breadth, volatility regime, drawdown depth |
| 5 | **Opportunity Set** | 10% | Quality of buying opportunities available *now* (catalysts, valuations, gaps) |

### Scoring guidance

- **0–20:** Excellent conditions. Add aggressively.
- **21–35:** Good conditions. Add on schedule.
- **36–50:** Mixed. Half pace.
- **51–65:** Cautious. Reduce adds.
- **66–80:** Defensive. Pause adds.
- **81–100:** Avoid adds. Capital preservation mode.

Each dimension's score must come with a **one-line rationale** citing
the specific data point driving it.

---

## Weighted Score → Kelly Factor

```
weighted_score = 0.30*market_val
               + 0.25*portfolio_val
               + 0.20*macro_liq
               + 0.15*technical
               + 0.10*opportunity
```

| Weighted Score | Kelly Factor | DCA Action (multiply by `state.yaml: monthly_dca`) |
|---|---|---|
| 0–20 | **1.00** | Deploy 100% of monthly DCA |
| 21–35 | **0.75** | Deploy 75% of monthly DCA |
| 36–50 | **0.50** | Deploy 50% of monthly DCA |
| 51–65 | **0.25** | Deploy 25% of monthly DCA |
| 66–80 | **0.10** | Deploy 10% of monthly DCA |
| 81–100 | **0.00** | Deploy 0% — all to Kelly Reserve |

(Illustratively: with the default `monthly_dca = $1,500`, the factors resolve to $1,500 / $1,125 / $750 / $375 / $150 / $0.)

**Kelly Reserve** = the accumulated undeployed portion of DCA. It can
later be deployed via:
- A Kelly Factor increase (status quo flow)
- A separate trigger (see Trigger Check — section 4 of Health Check)

---

## Worked Example

Suppose:
- Market Valuation: 45 (mixed)
- Portfolio Valuation: 30 (favorable — own holdings reasonably priced)
- Macro & Liquidity: 50 (neutral)
- Technical: 40 (slight weakness)
- Opportunity Set: 25 (good entry points available)

```
weighted = 0.30*45 + 0.25*30 + 0.20*50 + 0.15*40 + 0.10*25
         = 13.5 + 7.5 + 10.0 + 6.0 + 2.5
         = 39.5
```

Kelly Factor = **0.50** → Deploy 50% of your `state.yaml: monthly_dca` this month.
(Illustratively: with `monthly_dca = $1,500`, deploy $750 and reserve $750.)

---

## When To Recalculate

Kelly is **not** a fixed number. Recalculate when:
- Monthly DCA cycle begins (always)
- A major macro event occurs (Fed decision, geopolitical shock)
- Sleeve B caps are hit (matters for Portfolio Valuation)
- Sleeve D surgery happens (matters for Opportunity Set)
- NLV moves >5% in either direction

---

## Common Failure Modes

| Failure | Why it fails LIOS |
|---|---|
| "Markets feel expensive, skip DCA" | Use the model, not feel. If model says 0.50, deploy $750. |
| "Markets crashed, deploy everything" | A crash may trigger SR deployment, not Kelly. Don't confuse them. |
| "Tech is hot, weight Opportunity Set higher" | Weights are fixed. Adjust scores, not weights. |
| Ignoring Portfolio Valuation | A 20% position in a Sleeve B name is a Portfolio Valuation signal even if S&P is cheap. |

---

## Related
- See `02_capital_architecture.md` Sleeve C — where Kelly Reserve lives
- See `commands/kelly-recalc.md` — re-run scoring on demand
- See `06_health_check_structure.md` §3 — Kelly Dashboard placement in Health Check