> **Source:** Extracted verbatim from LIOS v1.5. Method — the Replacement Index governs Sleeve C surgery.

# Sleeve D — Replacement Index

**Component version:** v1.3 (unchanged in v1.4)
**Status:** Method (`constitution/method.md`) / dynamic scores
**Last updated:** 2026-07-25

---

## Purpose

The Replacement Index is the **only** legitimate mechanism for
Sleeve D surgery (entry, exit, swap). It exists to:

1. Prevent emotional / narrative-driven decisions
2. Force every D move to clear an objective bar
3. Make "should we replace X?" answerable from data, not vibes
4. Keep Sleeve D size disciplined (target per `state.yaml: sleeve_d.target_pct_of_total_nlv`)

---

## The Three Metrics

Each metric is scored 0-100 for both the **incumbent** (the D name
being evaluated for replacement) and the **candidate** (the proposed
replacement).

| Metric | Weight | What it measures |
|---|---|---|
| **Frontier Score** | 40% | How well-positioned is this name within the frontier tech theme? TAM capture, technological edge, moat trajectory |
| **Thesis Momentum** | 35% | Is the investment thesis *strengthening or weakening*? Earnings trajectory, guidance, competitive position, catalysts |
| **Diversification Value** | 25% | How much does this name *add* to the existing D portfolio? Low correlation to other D names, theme coverage |

Higher score = stronger on that metric. The composite is weighted.

---

## Composite Score

```
Replacement_Index = 0.40*frontier + 0.35*momentum + 0.25*diversification
```

Range 0-100. Used to rank all D names and any candidates.

---

## Replacement Approval Criteria

A replacement is approved **ONLY when ALL of the following are true**:

1. **Candidate Replacement Index > Incumbent Replacement Index** (composite > composite, with documented deltas)
2. **At least TWO of these four conditions are also met:**
   - Incumbent misses earnings OR issues weak guidance
   - Incumbent thesis weakens materially (product, market position, regulation)
   - Candidate trades at substantially more attractive valuation (e.g., >20% cheaper on forward P/E or P/S)
   - Candidate wave becomes significantly higher conviction (e.g., breakthrough, major contract win)

If criterion 1 is true but fewer than two of (2) hold → **hold and re-review next quarter.**
If criterion 2 has multiple trues but criterion 1 fails → **do not replace** (incumbent still has higher composite).

---

## Anti-Patterns (will be rejected)

| Trigger | Why it's not a valid replacement rationale |
|---|---|
| Short-term price movement alone | Use Replacement Index, not candles |
| Unrealized loss on incumbent | Sunk cost. Replacement is about forward expected value. |
| Excitement about a new name | Excitement is not a metric. Score it. |
| "It's been a while since we rotated" | Time-in-portfolio is not a metric. |
| One good earnings print on candidate | One quarter ≠ Thesis Momentum improvement. |

---

## Scores Are Dynamic

A score is an **assessment, not a permanent label**. Re-review after:

- Quarterly earnings (incumbent OR candidate)
- Material guidance changes
- Price moves > 20% in either direction
- Technology breakthroughs or major competitive shifts
- M&A events (acquirer / target)

If an incumbent's composite drops materially without a candidate
in play, that is itself a signal — flag in the Sleeve D Review,
do not auto-replace.

---

## Worked Example

Suppose Sleeve D holds **`<INCUMBENT>`** (incumbent). Consider replacing
with **`<CANDIDATE>`** (candidate).

| Metric | `<INCUMBENT>` | `<CANDIDATE>` |
|---|---|---|
| Frontier Score | 55 | 70 |
| Thesis Momentum | 40 | 65 |
| Diversification Value | 50 | 60 |
| **Composite** | **47.5** | **66.25** |

Criterion 1: ✅ Candidate > Incumbent.

Conditions:
- ✅ `<INCUMBENT>` missed last quarter earnings
- ❌ Thesis on `<INCUMBENT>` weakened only modestly
- ✅ `<CANDIDATE>` trades at ~30% cheaper forward P/E
- ❌ Wave conviction roughly equal

Criterion 2: ✅ Two conditions met (missed earnings + cheaper valuation).

**Verdict: Replacement approved.** Full Replacement Index workflow proceeds.

---

## Sleeve D Staging Reminder

Even after Replacement Index approval, **staging rules still apply**:

- **New name → Stage 1 (Watchlist)** unless already known
- **Stage 2 (Pilot)** typically ≤ 0.5% NLV
- **Stage 3 (Full position)** only after thesis confirmed via price action + earnings

SR deployment into D does NOT bypass staging. See `02_capital_architecture.md`.

---

## v1.4 — LOS as Complementary Layer

In LIOS v1.4+, **LOS** (LIOS Overall Score) is an **additional**
evaluation layer alongside the Replacement Index. The two are
complementary, not substitutes:

| Framework | What it measures |
|---|---|
| **Replacement Index** (L5) | Theme fit, thesis momentum, diversification value — surgical fit for Sleeve D |
| **LOS** (L11) | Overall investment quality (value / growth / quality / momentum) — universal company quality |

**Joint workflow for replacement:**

1. Candidate must first clear LOS gate:
   - LOS ≥ 75 typically required to enter Sleeve D consideration
   - LOS < 70 → automatic Pass; do not advance
2. Then Replacement Index approval (this document):
   - Composite superiority over incumbent
   - ≥ 2 of 4 qualitative conditions met
3. Then Sleeve D staging (Stage 1 → 2 → 3)

A candidate with high Frontier Score but weak LOS (e.g., LOS 65)
should **not** replace an incumbent without explicit IC rationale —
documented and disclosed in the Health Check.

See `08_research_intelligence_layer.md` for full LOS spec.
See `commands/los-replace.md` for the joint workflow command.

---

## Related
- See `02_capital_architecture.md` Sleeve D — staging rules, funding sources
- See `commands/sleeve-d-review.md` — run the framework on demand
- See `commands/los-replace.md` — joint LOS + Replacement Index workflow (v1.4)
- See `08_research_intelligence_layer.md` — LOS framework spec (v1.4)
- See `06_health_check_structure.md` §8, §11 — D Review and Frontier Scoreboard in Health Check