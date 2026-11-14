# Glidepath — Contribution Tilt Schedule

**Component version:** v4.1
**Status:** Method, not constitutional (targets are `state.yaml` parameters)
**Last updated:** 2026-11-15

---

## What this is

The glidepath sets how much of the portfolio may sit in risky assets as the
investor ages.

**It is not a de-risking schedule** unless the policy says so. In the default
policy it is a *contribution tilt*: it decides where new money goes. Existing
equity is not sold to reach a lower target.

That distinction matters. The previous version called this a glidepath while
forbidding the selling that would implement one. It was decoration. Renaming it
makes the mechanics honest.

---

## Two policies

| Policy | Rule | Equity weight at 75 |
|---|---|---|
| **Hard** | Targets binding. Rebalance sells equity to reach them | Target maintained |
| **Contribution tilt** (default) | Targets govern new money only. No de-risking sales | Drifts 10–20pp above target |

Choose **hard** if the capital will fund living expenses, where sequence risk
dominates. Choose **contribution tilt** if it is a legacy or longevity engine
that will not be drawn down.

Under the contribution-tilt policy, `state.yaml: glidepath.policy` must say
`contribution_tilt`. Under a hard policy, P4's prohibition on selling A into D/Z
is lifted, and the state file says so explicitly.

---

## Age bands

| Age | A | B | C | D | Z | Risky |
|---|---:|---:|---:|---:|---:|---:|
| 45–50 | 67% | 8% | 5% | 15% | 5% | **80%** |
| 50–55 | 64% | 6% | 5% | 20% | 5% | **75%** |
| 55–60 | 62% | 3% | 5% | 25% | 5% | **70%** |
| 60–65 | 60% | 0% | 5% | 30% | 5% | **65%** |
| 65+ | 55% | 0% | 5% | 35% | 5% | **60%** |

Sleeve B decays to zero. Individual conviction positions should not survive to
retirement: the research effort rises while the payoff horizon shortens, and a
single-name failure at 65 cannot be recovered by future earnings.

---

## The age-step problem

When the age band drops — say 80% risky to 75% at age 50 — the portfolio is
usually above the new ceiling on day one. Under the previous rules that was an
instant, silent breach of P2.

**Resolution:**

```
1  On the birthday that changes the band, the new targets take effect.
2  A 12-month grace period begins.
3  During grace, Top-up routes entirely to the defensive block.
4  At the end of grace, any remaining excess is closed by Rebalance.
```

Twelve months of Top-up at a realistic contribution rate rarely closes a 5pp
gap. That is fine — the point is that the rule is *stated*, with a defined end,
rather than being quietly ignored.

---

## Contribution step-up

```
DCA_next = DCA × (1 + min(salary growth, 5%))
```

Reviewed each January. Never decreased.

A flat nominal contribution loses roughly 40% of its purchasing power over 20
years. A 5% step-up produces about 65% more cumulative contributions than a flat
schedule at the same starting amount. This matters more than any allocation
choice in this document.

---

## After contributions stop

Under the contribution-tilt policy, once DCA stops the tilt has no lever. Equity
weight then drifts upward at roughly `w(1−w)(r_equity − r_defensive)` per year —
about 0.7pp per year at an 80/20 split and a 4.5% return spread.

Two consequences to state plainly, every cycle:

1. The glidepath **cannot** reduce equity exposure after contributions stop.
2. At age 65 the target says 60% but realised exposure may be 80%+.

If that is not acceptable, the policy must be **hard**, and P4's direction limit
must be lifted. There is no third option.

---

## Re-scoring

On life events only:

- health event
- loss of income or an emergency
- material change in net worth outside this portfolio
- change in dependants or bequest intent

No scheduled review. No market-driven review.

---

See also: `core/deployment.md`, `core/sleeves.md`, `constitution/constitution.md`.
