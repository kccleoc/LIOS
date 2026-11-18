# Glidepath (Part IV)

**LIOS v7.0.0 — adopted 2026-11-18. Supersedes v6.0.5 and every earlier version.**
**Status:** in force from adoption. This file is part of the v7.0.0 framework; where it disagrees with Part I (the constitution), Part I governs.

Policy: contribution tilt, always. Targets govern new money only.

---

## Part IV — Glidepath

The glidepath sets how much of the portfolio may sit in risky assets as the investor ages.

**Policy: contribution tilt, always.** Targets govern new money only. Existing equity is not sold to reach a lower target. There is no second policy and no switch: the "hard glidepath" variant is deleted, and with it the only mechanism that could lift P4′'s prohibition on selling A into C. A → C is always prohibited.

### What counts as risky

```
risky = A + B
defensive = C
```

**A is risky even when it holds a broad-index fund.** It is equity. Thematic ETFs inside A are the riskiest holdings in the book. **C is defensive.**

### Age bands

Sleeve B is **cap-only**, so it has no target row. **A's target is the residual:**

```
A_target = risky_target × NLV − B_actual_value
```

| Age | A (derived) | C (derived) | Risky (A+B) | B ceiling |
|---|---:|---:|---:|---:|
| 45–49 | 80 − B | 20 | **80%** | 10% |
| 50–54 | 75 − B | 25 | **75%** | 10% |
| 55–59 | 70 − B | 30 | **70%** | 10% |
| 60–64 | 65 − B | 35 | **65%** | 10% |
| 65+ | 60 − B | 40 | **60%** | 10% |

Two derived targets, one decision. The glidepath sets **one** number per age band — how much is risky — and both A and C fall out of it.

**Consequence worth stating:** an A→B transfer under IC decision is **gap-neutral**. Moving a dollar from A to B lowers A's value by one dollar and lowers A's target by the same dollar, because A's target is the residual. The transfer changes *concentration*, not *allocation*.

**B's ceiling does not decay.** It is 10% of NLV at every age, for life. The v6 rule that wound B down to 0% at age 60 is deleted: it contradicted P5′ (a B name is taken to zero only when its thesis breaks), and protection with age comes from the risky weight falling, not from liquidating the owner's conviction holdings.

### The age-step problem

When the age band drops — say 80% risky to 75% at age 50 — the portfolio is usually above the new ceiling on day one. Under a naive reading that is an instant, silent breach of P2′.

**Resolution:**

```
1  On the birthday that changes the band, the new targets take effect.
2  A 12-month grace period begins.
3  During grace, Top-up routes entirely to the defensive block.
4  At the end of grace, any remaining excess is reported as an OPEN de-risking gap.
```

Twelve months of Top-up at a realistic contribution rate rarely closes a 5pp gap. That is fine — the point is that the rule is *stated*, with a defined end, rather than being quietly ignored. **During the 12-month grace, a risky-band deviation caused by the band change is reported, not hard, until grace ends** — the 3×-tolerance hard rule (P3′) does not fire on a deviation the band change itself created. A deviation from any other cause is hard as usual. Under contribution tilt the new band is a **direction, not a deadline**: the gap closes when contributions and market moves close it, and it is reported every cycle until it does. There is no trade that closes it, because no permitted direction lowers risky weight. If a hard deadline is ever wanted, the only honest way to get one is an explicitly named, capped exception in the constitution — not a sentence in a method document that the direction table forbids.

### Contribution step-up

```
DCA_next = DCA × (1 + min(max(rate, 0%), 5%))
```

Reviewed each January. Never decreased. `rate` is a **fixed owner-declared rate of 0–5%**, recorded with the January review — the salary link is dropped: salary growth is self-reported with no verification source, and a formula that takes it would decrease the DCA when salary growth is negative. The January review is a **dated item in the execution queue** (`execution/pending.yaml`); if it is not completed by 31 January, it is reported as an open item at the next quarterly review. The step-up is recorded in the SSOT with the date and the figure used.

A flat nominal contribution loses roughly 40% of its purchasing power over 20 years. A 5% step-up produces about 65% more cumulative contributions than a flat schedule at the same starting amount. This matters more than any allocation choice in this document.

### After contributions stop

Under the contribution-tilt policy, once DCA stops the tilt has no lever. Equity weight then drifts upward at roughly `w(1−w)(r_equity − r_defensive)` per year — about 0.7pp per year at an 80/20 split and a 4.5% return spread.

Two consequences to state plainly, every cycle:

1. The glidepath **cannot** reduce equity exposure after contributions stop.
2. At age 65 the target says 60% but realised exposure may be 80%+.

There is no third option inside this framework. If that is not acceptable, the framework must be amended — not worked around.

### Reassessment — on life events only

On life events only:

- health event
- loss of income or an emergency
- material change in net worth outside this portfolio (declared by the owner with a dated record)
- change in dependants or bequest intent

No scheduled review. No market-driven review. A life event re-opens the risk discussion via constitutional amendment (major version bump + ADR) — it does not score anything, because there is no score.

---
