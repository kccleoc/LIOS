# 07 · The Constitution — P1 to P8

> **This file is a routing index.** The authoritative rules live in
> `constitution/constitution.md`. Do not duplicate rule text — duplicated rules
> drift.

**Component version:** v4.1
**Last updated:** 2026-11-15

---

| # | Rule | Source |
|---|---|---|
| **P1** | Five sleeves (A/B/C/D/Z), one denominator (NLV) | `core/sleeves.md` |
| **P2** | Targets from the glidepath; risky weight set by age | `core/glidepath.md` |
| **P3** | Two operations only — Top-up and Rebalance | `core/deployment.md` |
| **P4** | Rebalance direction: never sell A into D or Z | `core/deployment.md` |
| **P5** | Sleeve B exits only on thesis break, via a 30-day review | `core/sleeves.md` |
| **P6** | Caps outrank every rule | `core/sleeves.md` |
| **P7** | A cap breach is itself a Rebalance trigger | `core/deployment.md` |
| **P8** | Reports state dollars, confidence, fixed 16-section order | `constitution/method.md` |

---

## What is not a rule

LOS/LFS weights, band thresholds, Replacement Index weights, report templates,
communication style and glossary entries live in `constitution/method.md`. They
can be changed with a CHANGELOG entry and no version bump.

If it cannot be broken in a way that damages the portfolio, it is method.

---

## Amendment

A P-rule change requires a major version bump, an ADR, and a CHANGELOG entry.
There is no soft-amendment pathway.

Parameters in `state.yaml` — tickers, weights, caps, DCA, glidepath bands — are
portfolio-local and need no bump.

---

## History

L1–L23 were retired in v4.1.0. Twelve of them were method in constitutional
clothing; L4, L6, L7 were already dead; L19 and L21 were merged or deleted. The
mapping is in `constitution/constitution.md` § What replaced what.
