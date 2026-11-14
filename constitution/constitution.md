> **Source:** Replaces the L1–L23 "Locked Decisions" accumulated across v1.x–v3.0. Consolidated in v4.1.0.

# Portfolio Rules — P1 to P8

**Component version:** v4.1
**Status:** Constitutional. Changing any P-rule requires a major version bump.
**Last updated:** 2026-11-15

---

## What this file is

Eight rules. That is the whole constitution.

The previous structure carried twenty live "Locked Decisions" (L1–L23, with three
repealed). Twelve of them were not decisions at all — they were method
documentation wearing a constitutional costume: how to weight research scores,
how many sections a report has, how to write. Those now live in
`constitution/method.md` and can be changed with a changelog entry.

A rule belongs here only if breaking it would damage the portfolio. Everything
else is method.

---

## The Eight Rules

### P1 · Five sleeves, one denominator

The portfolio is partitioned into five sleeves:

| Sleeve | Holds | Risk role |
|---|---|---|
| **A** | Broad-index ETFs only | Systematic beta |
| **B** | 1–4 individual equities, equal weight at entry | Idiosyncratic alpha, capped |
| **C** | Discretionary rotation, winner-weighted | Active thematic, capped |
| **D** | Bonds, duration > 3 years, hedged | Ballast and ammunition |
| **Z** | Duration ≈ 0 — T-bills, money market, cash | Liquidity |

Every weight, cap, floor and ceiling is a percentage of **NLV**. No second
denominator exists. There is no `ABC` base.

A cash-like instrument may not sit inside A. A duration-bearing instrument may
not sit inside Z. Sleeves are defined by risk factor, not by issuer or ticker.

---

### P2 · Targets come from the glidepath; risky weight is set by age

Sleeve targets are read from the age band in `state.yaml: glidepath`.

**Total risky weight is set by age, not by opinion, not by the regime index, not
by a market view.** An investor who wants to change the equity level changes the
glidepath band and writes down why. Nothing else may move it.

---

### P3 · Money moves two ways only

| Operation | Job | Frequency | Sells? |
|---|---|---|---|
| **Top-up** | Allocate the monthly contribution | Monthly | **No** |
| **Rebalance** | Correct a band or cap breach | On breach | **Yes** |

There is no third operation. No discretionary channel, no reserve, no trigger
ladder, no override. Anything else is a rule violation.

A time-boxed **transition mandate** may exist (P3a) to migrate the portfolio from
one structure to another. It has a sunset date and cannot exceed the caps in P6.

---

### P4 · Rebalance may not de-risk the core

Rebalance may sell:

| Direction | Permitted? |
|---|---|
| Defensive → risky (D/Z → A/B/C) | ✅ The crash response |
| Defensive → defensive (Z → D) | ✅ Fixes a cash overweight |
| Risky → risky (B/C → A) | ✅ Fixes concentration |
| **A → D or Z** | ❌ **Prohibited** |

Selling core equity to buy bonds or cash is de-risking, and de-risking is what
the glidepath decides — not a rebalance. Under a hard glidepath this restriction
is lifted, and `state.yaml` says so explicitly.

---

### P5 · Sleeve B exits only on a thesis break

A Sleeve B holding is **exited** — taken to zero — only when its thesis breaks.
Price decline, drawdown, valuation and macro conditions are never exit triggers.

**LOS < 60 opens a 30-day deep-dive review.** The review must resolve to one of
exactly two outcomes:

1. a documented re-score of 60 or above, citing new evidence, or
2. exit.

No third outcome. No automatic extension. If the review is not completed within
30 days, the position exits.

**A weight trim is not an exit.** Trimming a position back to its band is risk
control and is always permitted.

---

### P6 · Caps outrank every rule

| Constraint | Default |
|---|---|
| Single-name effective exposure (direct + index-embedded) | 10% of NLV |
| Single-factor exposure | 25% of NLV |
| Duration sleeve | 15% of NLV |
| US-situs assets (non-US investor) | below the jurisdiction's estate-tax exemption |
| Satellite ceiling | 7% of NLV |

When a cap and any other rule disagree, **the cap wins**. A target that violates
a cap is invalid and must be re-solved.

Effective exposure includes index-embedded weight. A 6% direct position in a name
that is 6.5% of a 70% index sleeve is about 10.5% effective.

**US-situs:** stop adding when exposure reaches 80% of the ceiling, and buy
UCITS-domiciled instruments in preference to US-domiciled ones at all times.
This keeps the constraint from being breached by appreciation alone.

---

### P7 · A cap breach is itself a Rebalance trigger

No separate mechanism is needed. Any cap breach — name, factor, duration,
US-situs, sleeve ceiling — triggers Rebalance, and Rebalance resolves it under
the P4 direction limits.

If a breach cannot be cured within the P4 limits, it is escalated to the
Investment Committee as a structural problem, not silently tolerated.

---

### P8 · Reports state dollars, confidence, and fixed order

Every report:

- leads with the conclusion
- states dollar amounts, not percentages alone
- separates facts from assessments
- carries a confidence percentage on each recommendation
- follows the 16-section Health Check order when producing a full review

---

## Amendment

| Change | Requirement |
|---|---|
| Any P-rule | Major version bump, ADR, CHANGELOG entry |
| Anything in `constitution/method.md` | CHANGELOG entry only |
| Parameters in `state.yaml` (tickers, weights, caps, DCA) | No bump — they are portfolio-local |

There is no soft-amendment pathway.

---

## What replaced what

| Old | New |
|---|---|
| L1, P1 | **P1** |
| L2, L3 | **P2, P6** |
| P3 | **P3** |
| — (new) | **P4** |
| P5 | **P5** |
| P6 | **P6** |
| — (new) | **P7** |
| L9, L10 | **P8** |
| L5, L8, L11–L18 | `constitution/method.md` |
| L4, L6, L7, P2 | **deleted** |

---

See also: `constitution/method.md`, `constitution/governance.md`,
`core/deployment.md`, `core/sleeves.md`.
