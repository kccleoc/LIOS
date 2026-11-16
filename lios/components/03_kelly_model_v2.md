> **RETIRED — Kelly Model v2.0.** This file is named for the Kelly Model, and that
> model is **retired**. It was repealed in v3.0 and does not exist in v5: there is no
> Kelly Factor, no Kelly Reserve, no Kelly sizing and no Kelly multiplier on the
> contribution. Do not reintroduce it under any other name. The file is kept at this
> path for link stability and carries the **Regime Index**, a dashboard with no
> mechanical effect. The Kelly history below is retained as history only.

# 03 · Regime Index — Dashboard Only

> File kept at this path for link stability. The v2.x file here was the Kelly
> Model, repealed. It became a "Regime Index" in v3.0 and is now the
> **Regime Index**, with no mechanical effect.

**Component version:** v6
**Status:** Method (not constitutional)
**Last updated:** 2026-11-15

---

## The Index

```
RI = 0.30·MarketVal + 0.25·PortfolioVal + 0.20·MacroLiq
   + 0.15·Technical + 0.10·Opportunity
```

| # | Dimension | Weight |
|---|---|---:|
| 1 | Market Valuation | 30% |
| 2 | Portfolio Valuation | 25% |
| 3 | Macro & Liquidity | 20% |
| 4 | Technical | 15% |
| 5 | Opportunity | 10% |

0–100, higher = more bearish. Bands: Risk-on 0–35 · Constructive 36–50 ·
Cautious 51–65 · Defensive 66–100.

---

## What it does

Reports. That is all.

It does not size the contribution, shift sleeve targets, gate an operation, or
override a band. It gives you a consistent way to describe the environment and
notice change between cycles.

## What it may not do

| Attempt | Verdict |
|---|---|
| Cut or raise equity on the reading | ❌ P2′ — risky weight is set by age |
| Deploy or withhold capital | ❌ P3′ — Top-up is mechanical |
| Shift the A/B/C split | ❌ removed in v4.1; no such split exists in v5 |
| Size a position or move a sleeve target | ❌ P2′ — B is cap-only, A is the residual |
| Override a band or cap | ❌ P6′, P7′ |

---

## Why the shift was removed

Through v3.0 the index moved the A/B/C split by up to five percentage points.
That was removed for three reasons:

1. **It was market timing as composition.** Moving weight from high-beta single
   names into an index lowers portfolio beta by about 0.025 even with risky
   dollars unchanged.
2. **It caused selling.** Changing target weights made Rebalance sell names down
   to the new targets, contradicting "never acts by selling."
3. **It was untested.** A backtest of the split against a fixed allocation showed
   no reliable benefit.

---

## History: why it replaced the Kelly Factor

Retained as the record of the repeal. Kelly is not a live mechanism in v5 and no
part of this section is an instruction.

The Kelly Factor was not the Kelly criterion. It computed no edge, no payoff
ratio and no bankroll, and it sized a flow rather than wealth.

Kelly, computed properly, is `f* = (μ − r)/σ²` or `w* = (1/γ)Σ⁻¹(μ − r)` — both
linear in an unestimable μ. The plausible range for a broad index at current
valuations moves `f*` from roughly 0% to over 100% of wealth. Useful as a
diagnostic; unusable as a monthly controller.

**Disposition:** repealed in v3.0. In v5 the monthly contribution is allocated by
the Top-up rule alone (`core/deployment.md`): gap-driven across A, C, with B
excluded from the target set. Nothing multiplies it.

---

See also: `core/regime-index.md`, `core/deployment.md`,
`constitution/method.md`.
