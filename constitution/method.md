# Method — Scoring, Reports and Workflow

**Component version:** v4.1
**Status:** Method, not constitution. **Editable with a CHANGELOG entry. No version bump required.**
**Last updated:** 2026-11-15

---

## Why this file exists

The previous constitution carried twenty live rules. Twelve of them were method:
how to weight a research score, how many sections a report has, how to write.
Locking those as constitutional made every scoring tweak a version bump and made
the rule list unreadable.

They live here now. Change them freely; just record it.

Portfolio rules that can actually be broken live in
`constitution/constitution.md` (P1–P8).

---

## Research Intelligence Layer (LOS)

Every security researched receives four component scores, each 0–100:

| Score | Measures |
|---|---|
| **LVS** | Value — price versus intrinsic worth |
| **LGS** | Growth — durability and rate of the growth engine |
| **LQS** | Quality — capital allocation, governance, management integrity |
| **LMS** | Momentum — price and earnings trend |

Default weighting:

```
LOS = 0.35·LQS + 0.25·LGS + 0.25·LVS + 0.15·LMS
```

Deviations from the default weights must be disclosed with a one-line rationale.

### LOS bands

| LOS | Band | Action |
|---|---|---|
| 90–100 | Elite | Buy aggressively |
| 80–89 | Buy | Buy |
| 70–79 | Watch | Buy on a meaningful correction |
| 60–69 | Hold | Hold; no adds |
| **< 60** | Pass | **Opens the 30-day review under P5** |

Sleeve A index vehicles are exempt from LOS.

---

## Portfolio Fit Score (LFS)

Evaluated for every investment that reaches LOS ≥ 70. Measures how much the
investment improves *this* portfolio, not whether it is a good business.

| | High LFS | Low LFS |
|---|---|---|
| **High LOS** | Buy, in priority order | Buy smaller |
| **Low LOS** | Do not buy — LFS cannot rescue a poor investment | Do not buy |

When LOS values are within 5 points, prefer the higher LFS.

---

## Replacement Index (Sleeve C surgery)

| Metric | Weight | Measures |
|---|---:|---|
| Frontier | 40% | Position within the theme: TAM capture, moat trajectory |
| Momentum | 35% | Relative strength and thesis momentum |
| Diversification | 25% | Marginal risk reduction against the existing book |

Replacement requires composite superiority **plus** at least two of four
qualitative conditions. Sleeve C is the only sleeve that takes new ideas.

---

## Health Check — 15 sections, fixed order

1. Portfolio Health
2. Gap and Drift
3. Regime Index (dashboard)
4. Constraint Check
5. Sleeve A Review
6. Sleeve B Review
7. Sleeve C Review
8. Sleeve Z Review
9. Top-up Plan
10. Rebalance Status
11. Look-Through Concentration
12. Satellite Scoreboard
13. Execution Calendar
14. Execution Plan
15. Risk Assessment & CIO Resolution

Full specification: `core/health-check.md`.

---

## Communication standard

Conclusions first. Dollars always. Facts separated from assessments. Explicit
directives. Confidence percentage on every recommendation. No fluff, no
motivational language, no emoji.

---

## What may be changed here without a version bump

- LOS component weights and band thresholds
- LFS component definitions
- Replacement Index weights
- Report templates and section content
- Research workflow steps
- Glossary entries

## What may not

Anything in P1–P8. Those are constitutional.

---

See also: `constitution/constitution.md`, `core/research-layer.md`,
`core/execution-engine.md`, `core/replacement-index.md`.
