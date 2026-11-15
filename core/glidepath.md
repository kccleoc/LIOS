# Glidepath — Contribution Tilt Schedule

**Component version:** v5
**Status:** Method, not constitutional (targets are `state.yaml` parameters)
**Last updated:** 2026-11-16

---

## What this is

The glidepath sets how much of the portfolio may sit in risky assets as the
investor ages.

**It is not a de-risking schedule** unless the policy says so. In the default
policy it is a *contribution tilt*: it decides where new money goes. Existing
equity is not sold to reach a lower target.

---

## What counts as risky

```
risky = A + B
defensive = C + Z
```

**A is risky even when it holds a broad-index fund.** It is equity. Thematic ETFs
inside A are the riskiest holdings in the book.

**C is defensive.** It holds bonds with duration beyond three years. In v4.1 the
same sleeve letter held a satellite equity book, and counting it as risky was
correct then. In v5 it is not. Any formula that computes `(A + B + C) / NLV` is
wrong and will route every future contribution into bonds.

---

## Two policies

| Policy | Rule | Equity weight at 75 |
|---|---|---|
| **Hard** | Targets binding. Rebalance sells equity to reach them | Target maintained |
| **Contribution tilt** (default) | Targets govern new money only. No de-risking sales | Drifts 10–20pp above target |

Choose **hard** if the capital will fund living expenses, where sequence risk
dominates. Choose **contribution tilt** if it is a legacy or longevity engine that
will not be drawn down.

Under the contribution-tilt policy, `state.yaml: glidepath.policy` must say
`contribution_tilt`. Under a hard policy, P4′'s prohibition on selling A into C/Z
is lifted, and the state file says so explicitly.

---

## Age bands

Sleeve B is **cap-only**, so it has no target row. **A's target is the residual:**

```
A_target = risky_target − B_actual
```

That is what keeps `A + B + C + Z = 100%` while B floats anywhere from 0 to its
ceiling. At B = 0 the sleeve takes the whole risky budget; at B = ceiling it takes
the ceiling's share.

| Age | A (residual) | C | Z | Risky (A+B) | B ceiling |
|---|---:|---:|---:|---:|---:|
| 45–50 | 80 − B | 15 | 5 | **80%** | 10% |
| 50–55 | 75 − B | 20 | 5 | **75%** | 8% |
| 55–60 | 70 − B | 25 | 5 | **70%** | 5% |
| 60–65 | 65 − B | 30 | 5 | **65%** | 0% |
| 65+ | 60 − B | 35 | 5 | **60%** | 0% |

**Consequence worth stating:** an A→B transfer under IC decision is
**gap-neutral**. Moving a dollar from A to B lowers A's value by one dollar and
lowers A's target by the same dollar, because A's target is the residual. The
transfer changes *concentration*, not *allocation*. Any rule that treated A's
target as fixed would double-count the transfer and order a second trade.

B's ceiling decays with the band:

| Age | B ceiling |
|---|---:|
| 45–50 | 10% |
| 50–55 | 8% |
| 55–60 | 5% |
| 60–65 | 0% |
| 65+ | 0% |

Satellites should not survive to retirement: the research effort rises while the
payoff horizon shortens, and a single-name failure at 65 cannot be recovered by
future earnings. At 60 the ceiling reaches zero and B is wound down by ordinary
Rebalance into A.

---

## The age-step problem

When the age band drops — say 80% risky to 75% at age 50 — the portfolio is usually
above the new ceiling on day one. Under a naive reading that is an instant, silent
breach of P2′.

**Resolution:**

```
1  On the birthday that changes the band, the new targets take effect.
2  A 12-month grace period begins.
3  During grace, Top-up routes entirely to the defensive block.
4  At the end of grace, any remaining excess is reported as an OPEN de-risking gap.
```

Twelve months of Top-up at a realistic contribution rate rarely closes a 5pp gap.
That is fine — the point is that the rule is *stated*, with a defined end, rather
than being quietly ignored.

**What step 4 used to say, and why it could not be obeyed.** It said the excess "is
closed by Rebalance". Under `glidepath.policy: contribution_tilt` no permitted direction
lowers risky weight: `A → C` and `A → Z` are prohibited by P4′ because that is de-risking,
`B → A` leaves risky unchanged, `C`/`Z → A` raises it, and `Z → C` leaves it unchanged. A
grace-end Rebalance would therefore fire and have nothing legal to execute — a rule that
looks enforceable and is not, which is worse than an honest gap.

Under `contribution_tilt` the new band is a **direction, not a deadline**. The gap closes
when contributions and market moves close it, and it is reported every cycle until it does.
Under `glidepath.policy: hard` the A→C/Z restriction lifts and step 4 becomes a real trade.
If a hard deadline is ever wanted under `contribution_tilt`, the only honest way to get one
is an explicitly named, capped exception in the constitution — not a sentence in a method
document that the direction table forbids. See `core/deployment.md`, Escalation.

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

If that is not acceptable, the policy must be **hard**, and P4′'s direction limit
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
