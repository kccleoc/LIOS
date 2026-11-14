# 03 · Bull–Bear Index — Scoring Reference

> **File kept at this path for link stability.** The v2.x file here was the
> Kelly Model. That model is repealed (L4). This file now documents its
> replacement.

**Component version:** v3.0
**Status:** Locked Decision (L21)
**Last updated:** 2026-11-14

---

## The Index

```
BBI = 0.30·MarketVal + 0.25·PortfolioVal + 0.20·MacroLiq
    + 0.15·Technical + 0.10·Opportunity
```

0–100. **Higher = more bearish / more hostile to deployment.**

| # | Dimension | Weight | What it measures |
|---|---|---:|---|
| 1 | Market Valuation | 30% | Broad equity valuations — P/E, CAPE, ERP, breadth |
| 2 | Portfolio Valuation | 25% | Own holdings vs their history, growth durability |
| 3 | Macro & Liquidity | 20% | Rates, credit spreads, liquidity, policy, USD |
| 4 | Technical Conditions | 15% | Trend, momentum, breadth, volatility, drawdown |
| 5 | Opportunity Set | 10% | Quality of entry points available now |

Each dimension carries a score and a one-line rationale citing a specific data
point.

---

## Bands

| BBI | Band | A | B | C | D | Z | Risky |
|---|---|---:|---:|---:|---:|---:|---:|
| 0–35 | Risk-On | 65% | 9% | 6% | 15% | 5% | 80% |
| 36–50 | Constructive | 66% | 8.5% | 5.5% | 15% | 5% | 80% |
| 51–65 | Cautious | 70% | 6% | 4% | 15% | 5% | 80% |
| 66–100 | Defensive | 73% | 4% | 3% | 15% | 5% | 80% |

Total risky is identical in every band. Only the passive/active split moves.

---

## How the Band Is Used

- The band shifts the A/B/C split inside the risky block.
- It is executed by the **DCA waterfall** — a step-down starves B and C of new
  contributions. It never triggers a sale.
- At most **one band step per quarter**.

---

## Prohibitions

| Attempt | Verdict |
|---|---|
| Use BBI to cut total equity | ❌ L21 — total risky is fixed |
| Use BBI to deploy a reserve | ❌ No reserve exists |
| Skip a contribution because BBI is high | ❌ Rule 1 is mechanical |
| Override the band by discretion | ❌ L21 |

---

## Why This Replaced the Kelly Factor

The Kelly Factor was not the Kelly criterion. It computed no edge, no payoff
ratio and no bankroll term, and it sized a flow rather than wealth. It also
ignored the distance between the portfolio and its target.

Kelly, computed properly, is `f* = (μ − r)/σ²` or `w* = (1/γ)Σ⁻¹(μ − r)` — both
linear in an unestimable μ. The plausible range for a broad equity index at
current valuations moves `f*` from roughly 0% to over 100% of wealth. That is
useful as a diagnostic and unusable as a monthly controller, which is why v3.0
keeps only an ordinal gauge.

---

See also: `core/regime-index.md`, `core/deployment.md`, `scores/Regime.md`.
