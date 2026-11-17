# Method — Scoring, Reports and Workflow

**Component version:** v6
**Status:** Method, not constitution. **Editable with a CHANGELOG entry. No version bump required.**
**Last updated:** 2026-11-17

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

## Health Check — 14 sections, fixed order

1. Portfolio Health
2. Gap and Drift
3. Regime Index (dashboard)
4. Constraint Check
5. Sleeve A Review
6. Sleeve B Review
7. Sleeve C Review
8. Top-up Plan
9. Rebalance Status
10. Look-Through Concentration
11. Satellite Scoreboard
12. Execution Calendar
13. Execution Plan
14. Risk Assessment & CIO Resolution

Full specification: `core/health-check.md`.

---

## Quarterly Review — fixed calendar and standing agenda

**Dates.** The Quarterly Review is held on the **last Saturday of February, May, August
and November**. Under this rule the reviews are Saturday **27 February 2027**, Saturday
**29 May 2027**, Saturday **28 August 2027** and Saturday **27 November 2027**, then the
same rule every year.

**Marks.** The review is struck on **quarter-end closing prices** — 31 December, 31 March,
30 June or 30 September, whichever falls immediately before the review — and on the
**latest reported earnings** for each holding. A review that marks the book at the prices
of the week it is held tests the caps against a number nobody agreed to.

**Why a weekend.** By the last Saturday of those months the reporting season is complete
for the portfolio's holdings, quarter-end prices are available, and the markets are shut,
so no decision taken at the review is a decision taken at a price the review created.

**Standing agenda** — the same items, in this order, every quarter:

1. **Recompute every cap at quarter-end prices.** Sleeve B at or under 10% of NLV;
   look-through single-company at or under 5%; single-sector at or under 25% on GICS
   industry group; US-situs below the ceiling; Sleeve C average effective duration
   between 1 and 6 years, with no single holding above 10 years.
2. **Re-score LOS for every B name.** A name below 60 opens the 30-day review under P5′.
3. **Decide the B name count** against `names_max`. The cap is forward-looking: it refuses
   the next addition, and a count already above the limit is settled here, never by a
   forced sale. This is the only place that count is resolved.
4. **Check the DCA Top-up tilt.** The monthly contribution is the only instrument that
   closes a risky-weight gap, so the tilt is reviewed, not the gap.

**Every decision is an Investment Committee record. No record, no funding.** The record
names the candidate and the sleeve, the LOS and Replacement Index against the weakest
incumbent, the dollar amount free of the ceiling and the look-through it creates, the
alternative that was rejected, and the condition that would reverse the decision.

**Hard caps are never deferred to a review.** Look-through, single-sector, US-situs and the
single-holding duration cap fire the moment they are breached (P7′); a review cannot
schedule them. Only soft limits — sleeve bands, the risky band, the B ceiling and a B count
already above `names_max` — wait for this date.

**Scope.** This section fixes dates and an agenda. It changes no cap, target, tolerance,
sleeve or operation, and it may not be used to introduce one: those live in
`constitution/constitution.md` and move only by the amendment rule there.

**Pinned.** This section is the operative definition of "quarterly" in P3′ and P7′, so it
is not free method text like the rest of this file. Changing a date, the marks convention,
or the order of the agenda is a **constitutional amendment — major version bump plus an
ADR — not a CHANGELOG entry.** The latitude in the header covers everything else here; it
does not reach this section. Without that lock, the meaning of a constitutional term could
be moved by the cheapest edit in the system, which is the loophole the Amendment table
exists to close.

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
