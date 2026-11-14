> **Source:** Replaces the Kelly Factor (repealed). The five dimensions and their
> weights are unchanged.

# Regime Index — Dashboard Only

**Component version:** v4.1
**Status:** Method, not constitutional. **This index has no mechanical effect on the portfolio.**
**Last updated:** 2026-11-15

---

## What this is

A 0–100 reading of how hostile current conditions look, reported every cycle.

**It does not size anything, gate anything, or move any money.** It replaced a
"Kelly Factor" that multiplied the monthly contribution — a job it was never
equipped to do. In v4.1 it does the only thing a score is good for: giving you a
consistent way to describe the environment and notice change.

---

## The Index

```
RI = 0.30·MarketVal + 0.25·PortfolioVal + 0.20·MacroLiq
   + 0.15·Technical + 0.10·Opportunity
```

| # | Dimension | Weight | What it measures |
|---|---|---:|---|
| 1 | Market Valuation | 30% | Broad valuations — P/E, CAPE, ERP, breadth |
| 2 | Portfolio Valuation | 25% | Own holdings versus their history |
| 3 | Macro & Liquidity | 20% | Rates, credit spreads, liquidity, policy, USD |
| 4 | Technical | 15% | Trend, momentum, breadth, volatility, drawdown |
| 5 | Opportunity | 10% | Quality of entry points available now |

0–100. **Higher = more bearish / more hostile.**

| Range | Reading |
|---|---|
| 0–35 | Risk-on |
| 36–50 | Constructive |
| 51–65 | Cautious |
| 66–100 | Defensive |

Each dimension carries a one-line rationale citing a specific data point.

---

## What it may not do

| Attempt | Verdict |
|---|---|
| "Index is 75, cut equity exposure" | ❌ P2 — risky weight is set by age |
| "Index improved, deploy more" | ❌ Top-up is mechanical (P3) |
| "Index is 70, skip the contribution" | ❌ P3 |
| "Index says defensive, override a band" | ❌ P7 — bands and caps govern |
| "Shift the A/B/C split on the index" | ❌ removed in v4.1 |

The last row is the important one. In v3.0 the index shifted the A/B/C split by
up to five percentage points within the risky block. That was removed because:

1. It was market timing expressed as *composition* rather than *level* — a shift
   from high-beta single names into an index lowers portfolio beta by roughly
   0.025 even with the risky dollars unchanged.
2. Changing target weights caused Rebalance to sell single names down to the new
   targets, contradicting "never acts by selling."
3. It made every band and cap depend on the index value, which is untested.

A backtest of the split against a fixed allocation showed no reliable benefit at
any horizon tested. It is gone.

---

## Reporting

```
Regime Index: 58.2 (Cautious)     prior: 60.1     change: none
  Market Valuation     68   S&P −2.9% off high; 10Y at 5.07% compresses ERP
  Portfolio Valuation  50   top holding ~17x forward, a decade low; weight reduced
  Macro & Liquidity    70   credit calm, export-control overhang repriced
  Technical            30   six down weeks, below all MAs
  Opportunity          68   entries appearing, but every candidate fails LOS 80
```

That is the entire output. No allocation follows from it.

---

## When it is updated

Monthly, and after a material macro event. If a dimension cannot be scored from
available data, say so — do not extrapolate.

---

See also: `core/deployment.md`, `constitution/constitution.md`,
`constitution/method.md`.
