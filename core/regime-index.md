> **Source:** Replaces the Kelly Factor (L4 — repealed in v3.0). The five
> dimensions and their weights are unchanged; the output and its role are not.

# Bull–Bear Index (BBI)

**Component version:** v3.0
**Status:** Locked Decision (L21)
**Last updated:** 2026-11-14

---

## What changed and why

Kelly Model v2.0 converted a 0–100 conditions score into a 0–1 "Kelly Factor"
and multiplied the monthly DCA by it. Two problems:

1. **It was not Kelly.** No edge, payoff ratio or bankroll term was ever
   computed. A risk-appetite dial was wearing the name of an optimisation rule.
2. **It ignored the distance to target.** A book 5pp underweight received the
   same deployment as a book on target.

v3.0 keeps the scoring (it is a useful gauge) and removes the sizing job.
The index is now reported as **0–100, higher = more bearish**, and its only
mechanical effect is to shift the internal split of the risky block.

---

## The Index

```
BBI = 0.30·MarketVal + 0.25·PortfolioVal + 0.20·MacroLiq
    + 0.15·Technical + 0.10·Opportunity
```

| # | Dimension | Weight | What it measures |
|---|---|---:|---|
| 1 | Market Valuation | 30% | Broad equity valuations — P/E, CAPE, equity risk premium, breadth |
| 2 | Portfolio Valuation | 25% | Valuation of the investor's own holdings vs their history |
| 3 | Macro & Liquidity | 20% | Rates, credit spreads, liquidity, policy posture, USD |
| 4 | Technical Conditions | 15% | Trend, momentum, breadth, volatility regime, drawdown depth |
| 5 | Opportunity Set | 10% | Quality of entry points available *now* |

Every dimension is scored 0–100 where **higher = more bearish / more hostile to
deployment**. Each score carries a one-line rationale citing the data point that
drives it.

### Scoring guidance

| Score | Reading |
|---|---|
| 0–20 | Very favourable. Cheap, liquid, trending. |
| 21–35 | Favourable. |
| 36–50 | Constructive / neutral. |
| 51–65 | Cautious. Expensive or deteriorating. |
| 66–80 | Defensive. |
| 81–100 | Capital-preservation regime. |

---

## Bands and the Mechanical Effect

BBI never changes total risky exposure. It shifts the **A/B/C split inside a
fixed risky budget**.

| BBI | Band | A | B | C | D | Z | Risky |
|---|---|---:|---:|---:|---:|---:|---:|
| 0–35 | Risk-On | 65% | 9% | 6% | 15% | 5% | **80%** |
| 36–50 | Constructive | 66% | 8.5% | 5.5% | 15% | 5% | **80%** |
| 51–65 | Cautious | 70% | 6% | 4% | 15% | 5% | **80%** |
| 66–100 | Defensive | 73% | 4% | 3% | 15% | 5% | **80%** |

D and Z are constant. The risky block is constant. Only the split between
passive index and active bets moves.

**Direction of the shift:** as the regime worsens, the model reduces *active
risk* — the names most likely to fail alongside a company-specific event and
the ones hardest to hold with conviction. It does **not** reduce market risk,
because a long-horizon accumulator should not time the market.

### Implementation

- The shift is executed by the **DCA waterfall** (`core/deployment.md`, Rule 1).
- A step-down **starves** B and C of new money. It **never sells**.
- At most one band step per quarter. The index is re-scored monthly; the
  allocation changes slowly by design.

Effect size on a $1M book: a move from Constructive to Cautious shifts about
$30,000 of target weight from B/C to A. Executed by flow, that is 3–6 months of
DCA with zero transactions.

---

## What the Index May Not Do

| Attempt | Verdict |
|---|---|
| "BBI is 80, move everything to cash" | ❌ Total risky is fixed at 80%. Use the target split |
| "BBI improved, deploy the reserve" | ❌ No reserve exists. Only Rule 2 or Rule 3 move money |
| "BBI is 70, skip this month's DCA" | ❌ Rule 1 is mechanical |
| "Override the band because I disagree" | ❌ L21 |
| "Add a sixth dimension" | ❌ L21 — weights and dimensions are fixed |

---

## Reporting

Every Health Check reports:

```
BBI: 60.1 (Cautious)
  Market Valuation     66
  Portfolio Valuation  58
  Macro & Liquidity    68
  Technical            40
  Opportunity          62
Band change vs prior cycle: none
```

Plus one line: **what the band implies for B and C flow this cycle**, not a
dollar deployment figure.

---

See also: `core/sleeves.md`, `core/deployment.md`, `scores/Regime.md`.
