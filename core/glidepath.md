# Glidepath

**Component version:** v3.0
**Status:** Parameter document (not a Locked Decision)
**Last updated:** 2026-11-14

---

## Purpose

The glidepath sets **how much of the portfolio is allowed to sit in risky
assets as the investor ages**. It is expressed as age bands, not as market
signals.

It answers one question only: *at what rate does new money stop going into
equities?*

---

## Two Policies — Pick One

| Policy | Rule | Effect at age 75 |
|---|---|---|
| **Hard glidepath** | Targets are binding. Rebalance sells equities to hit them. | Target weight maintained exactly |
| **Drift-accepted glidepath** (default) | Targets are **floors for new money only**. No rebalancing sells to de-risk. Equities drift upward. | Equity weight may exceed target by 10–20pp |

The default is **drift-accepted**. Choose it when the portfolio is a
generational engine that will not be drawn down. Choose a hard glidepath when
the capital will fund living expenses, in which case sequence risk dominates.

Under a drift-accepted glidepath, the glidepath is implemented entirely by the
DCA waterfall (`core/deployment.md`, Rule 1). As the equity target falls, a
larger share of each contribution goes to D and Z; existing equity is untouched.

---

## Age Bands

| Age | A | B | C | D | Z | Risky |
|---|---:|---:|---:|---:|---:|---:|
| 45–50 | 67% | 8% | 5% | 15% | 5% | **80%** |
| 50–55 | 64% | 6% | 5% | 20% | 5% | **75%** |
| 55–60 | 62% | 3% | 5% | 25% | 5% | **70%** |
| 60–65 | 60% | 0% | 5% | 30% | 5% | **65%** |
| 65+ | 55% | 0% | 5% | 35% | 5% | **60%** |

**Sleeve B decays to zero.** Individual conviction positions should not survive
to retirement: the research effort required to hold them rises while the payoff
horizon shortens, and a single-name failure at 65 cannot be recovered by future
earnings.

The band values are parameters in `state.yaml: glidepath`. The table above is
the default.

---

## Interaction With the Regime Index

The glidepath and the BBI both adjust A/B/C, and they can conflict.

```
Resolution order:
1. Glidepath sets the total risky weight for the age band.
2. BBI distributes the *active* portion (B + C) inside that risky weight.
3. BBI may not raise total risky above the glidepath band.
```

Example at age 52 (glidepath risky = 75%, BBI Defensive):

| | Value |
|---|---:|
| Glidepath risky | 75% |
| BBI Defensive active share | B 4%, C 3% |
| A absorbs the rest | 68% |
| D + Z | 25% |

---

## DCA Step-Up

Contributions grow with income so the engine keeps pace with the investor's
earning power.

```
Rule:   DCA_next = DCA × (1 + min(nominal salary growth, 5%))
        Never decrease. Review each January.
Floor:  contributor-defined minimum step (e.g. one currency unit per month).
```

A flat nominal contribution loses roughly 40% of its purchasing power over 20
years. At an illustrative 5% step-up, cumulative contributions over 20 years
are **65% larger** than a flat schedule at the same starting amount. This
matters more than any allocation choice in this document.

---

## Decumulation

Two modes:

| Mode | Rule |
|---|---|
| **No-sell (default)** | Contributions stop at retirement. No withdrawals. Equity drifts up. Portfolio runs until it is spent on care, bequest or opportunity |
| **Drawdown** | Withdrawals come from Z, then D, then A. Never sell A to fund lifestyle while D is above floor |

Under no-sell, state explicitly in the Health Check that a 40% drawdown at
age 75 is accepted and cannot be recovered by labour income. If that sentence
is uncomfortable, the glidepath is too aggressive and should be moved one band.

---

## Re-Scoring

The glidepath is re-scored on **life events only**:

- health event
- loss of income / emergency
- material change in total net worth outside this portfolio
- change in dependants or bequest intent

No scheduled review. No market-driven review.

---

See also: `core/sleeves.md`, `core/deployment.md`, `core/regime-index.md`.
