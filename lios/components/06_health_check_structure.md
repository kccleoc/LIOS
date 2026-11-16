> **Source:** Rebuilt for three sleeves in v5. Replaces the v4.1 sixteen-section order.
> Structure is constitutional (P8′); content is method.

# 06 · Health Check Structure (15 Sections)

**Component version:** v6
**Status:** Constitutional (P8′) — structure; method — content
**Last updated:** 2026-11-15

---

## Purpose

The Health Check is the canonical output format for a full session review.

**Always 14 sections, always this order.** Deviating breaks cross-session
comparability.

**Why 15 and not 16.** v4.1 carried a separate Sleeve D Review. v5 deletes sleeve D
and moves duration into C, so C's review carries ballast and ammunition. A section
that reports an empty sleeve is a section nobody reads.

The authoritative specification is `core/health-check.md`. This file is a reference
summary.

---

## The 15 Sections

| # | Section | Output |
|---|---|---|
| 1 | Portfolio Health | NLV, sleeve totals vs targets, reconciliation |
| 2 | Gap & Drift | One table — gap and drift are the same number |
| 3 | Regime Index (dashboard) | Five scores, weighted value, band — no action |
| 4 | Constraint Check | Look-through name, sector, duration, risky, US-situs |
| 5 | Sleeve A Review | Holdings, view statements, within-A split, thematic drift |
| 6 | Sleeve B Review | LOS, thesis-exit status, ceiling status, replacement ranking |
| 7 | Sleeve C Review | Composition, duration table, money duration, band status, ammunition |
| 8 | Top-up Plan | Allocation in dollars — A and C. Never B |
| 9 | Rebalance Status | Triggered? Hard or soft? Executed? Escalated? |
| 10 | Look-Through Concentration | Name and sector exposure through every wrapper |
| 11 | Satellite Scoreboard | B names by Replacement Index, plus candidates |
| 12 | Execution Calendar | Earnings, macro, index events |
| 13 | Execution Plan | Buy / Hold / Reduce / Exit, with dollars |
| 14 | Risk Assessment & CIO Resolution | Drawdown ladder in dollars, then the decision |

---

## Section 1 — Portfolio Health

Total NLV, sleeve-by-sleeve value and weight, reconciliation delta. Headline facts
only: where we are, what changed, what matters next.

## Section 2 — Gap & Drift

One table: sleeve, value, target dollars, gap dollars, gap in pp, band status.

```
G_s = w_s × NLV − V_s
```

Gap and drift are the same number; never compute both. **B appears with a ceiling, not
a target** — its distance to the ceiling is not fundable. Gaps over {A, C} sum to
zero. Bands follow `band_s = clamp(0.5 × target_s, 2pp, 8pp)`.

## Section 3 — Regime Index (dashboard)

```
RI = 0.30·MarketVal + 0.25·PortfolioVal + 0.20·MacroLiq
   + 0.15·Technical + 0.10·Opportunity
```

0–100, higher = more bearish. Bands: Risk-on 0–35 · Constructive 36–50 ·
Cautious 51–65 · Defensive 66–100.

**The index has no mechanical effect.** It sizes nothing, shifts no sleeve target and
gates no operation. The v4.1 behaviour — moving the A/B/C split inside a fixed risky
budget — is deleted. Total risky weight is set by the glidepath, not by a reading
(P2′). State this explicitly every cycle.

## Section 4 — Constraint Check

Compute **look-through** exposure: direct weight, plus fund look-through, plus
index-embedded weight. A direct position inside the cap can still breach on an
effective basis.

Check, in dollars and in percent: look-through single-company, single-sector,
duration (C), total risky weight `(A + B) / NLV`, US-situs against its ceiling, and
Sleeve B against its ceiling.

## Section 5 — Sleeve A Review

Per holding: dollars, percent of A, percent of NLV. Within-A split of core beta
versus thematic against `sleeves.a.thematic_max_pct`. For each thematic holding, the
written view statement, its horizon, and whether the invalidation condition has been
met. Look-through note naming the largest companies the sleeve owns across funds.
Verdict per holding, citing the P5′ clause.

## Section 6 — Sleeve B Review

Per holding: dollars, percent of NLV, LOS and its four components, days since last
score. Ceiling status against `sleeves.b.ceiling_pct` with the dollar overage. Count
of names below LOS 60 and any open 30-day review. Replacement ranking by index, with
the weakest named. State that B received no Top-up this month, by rule.

## Section 7 — Sleeve C Review

Total, percent of NLV, target, gap. Duration in years, currency hedge ratio,
instrument list with maturity and credit quality. Build progress if C is below
target, and the Top-up path that closes it. Ammunition available: the amount C could
deploy without leaving its band. No ranking and no laggard cut list — C is bonds.

## Section 8 — Top-up Plan

Show `NLV'`, the gap per sleeve, `ΣG⁺` and the allocation in dollars across A, C and
Z only. State `risky_now = (A + B) / NLV` and whether the risky-cap fallback applied
— that fallback allocates to C. State that Sleeve B receives nothing by rule:
B is excluded from the target set and cannot receive a share. If a Rebalance
triggered **and executed a trade**, say that it absorbs the contribution and Top-up
is not run separately.

## Section 10 — Rebalance Status

Any hard-cap breach: which one, how large in dollars, cure plan. Any soft breach:
which sleeve, how far beyond `rebalance_tolerance_pct`, and whether the quarterly
review is due. Whether a triggered Rebalance executed a trade. Escalation status if a
breach cannot be cured within the P4′ direction limits.

## Section 11 — Look-Through Concentration

Top ten underlying companies across the whole book, in dollars and percent of NLV,
each against the look-through cap. Sector totals against the sector cap. Name the
double-counting explicitly: which companies appear in more than one wrapper, and at
what combined weight. Broad-index funds are included — index-embedded weight counts.

## Section 12 — Satellite Scoreboard

B names ranked by Replacement Index with the weakest flagged. Watchlist candidates
with LOS and LFS. The replacement test: composite superiority plus two of four
qualitative conditions, size-neutral. Whether any candidate has IC approval to be
funded from A.

## Section 13 — Execution Calendar

Earnings dates for B names. Index rebalance and distribution dates for A holdings.
Macro events and scheduled reviews. The next quarterly Rebalance review date.

## Section 14 — Execution Plan

Per line: Buy / Hold / Reduce / Exit, with dollars and share counts. Every action
cites the rule that produced it — band, cap, Top-up, or the transition mandate. An
action with no rule citation is deleted from the plan.

## Section 15 — Risk Assessment & CIO Resolution

Restate the drawdown ladder **in dollars**: ordinary bad quarter, bad quarter, bad
year, severe drawdown. Add factor and concentration risk in dollars. If any number is
unacceptable, the fix is the glidepath — not a market call. Then one paragraph: the
decision, the reasoning, and a confidence percentage.

---

## Writing Rules

- Conclusions first, reasoning second
- Dollar amounts always, never percentages alone
- Facts separated from assessments
- Confidence % on sections 5, 6, 7, 14 and 15
- No skipped sections — "no action" plus a one-line rationale is acceptable
- No Kelly Factor, no Strategic Reserve, no `ABC` denominator, no Regime Index acting
  on allocation
- No Sleeve D. If a section references it, the section is stale

---

See also: `core/health-check.md`, `core/deployment.md`, `core/regime-index.md`,
`02_capital_architecture.md`.
