> **Source:** Adapted from LIOS v1.5. The Replacement Index governs Sleeve B replacement.

# Sleeve B — Replacement Index

**Component version:** v5
**Status:** Method (`constitution/method.md`) / dynamic scores
**Last updated:** 2026-11-15

---

## Purpose

The Replacement Index is the **only** legitimate mechanism for
Sleeve B replacement. It exists to:

1. Prevent emotional / narrative-driven decisions
2. Force every B replacement to clear an objective bar
3. Make "should we replace X?" answerable from data, not vibes
4. Keep Sleeve B within its ceiling (`state.yaml: sleeves.b.ceiling_pct`)

---

## The Three Metrics

Each metric is scored 0-100 for both the **incumbent** (the B name
being evaluated for replacement) and the **candidate** (the proposed
replacement).

| Metric | Weight | What it measures |
|---|---|---|
| **Frontier Score** | 40% | How well-positioned is this name within the frontier tech theme? TAM capture, technological edge, moat trajectory |
| **Thesis Momentum** | 35% | Is the investment thesis *strengthening or weakening*? Earnings trajectory, guidance, competitive position, catalysts |
| **Diversification Value** | 25% | How much does this name *add* to the existing B portfolio? Low correlation to other B names, theme coverage |

Higher score = stronger on that metric. The composite is weighted.

---

## Composite Score

```
Replacement_Index = 0.40*frontier + 0.35*momentum + 0.25*diversification
```

Range 0-100. Used to rank all B names and candidates.

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
in play, that is itself a signal — flag in the Sleeve B Review,
do not auto-replace.

---

## Worked Example

Suppose Sleeve B holds **`<INCUMBENT>`** (incumbent). Consider replacing
with **`<CANDIDATE>`** (candidate).

| Metric | `<INCUMBENT>` | `<CANDIDATE>` |
|---|---|---|
| Frontier Score | 55 | 70 |
| Thesis Momentum | 40 | 65 |
| Diversification Value | 50 | 60 |
| **Composite** | **47.5** | **66.25** |

Criterion 1: PASS — Candidate > Incumbent.

Conditions:
- PASS — `<INCUMBENT>` missed last quarter earnings
- FAIL — Thesis on `<INCUMBENT>` weakened only modestly
- PASS — `<CANDIDATE>` trades at ~30% cheaper forward P/E
- FAIL — Wave conviction roughly equal

Criterion 2: PASS — Two conditions met (missed earnings + cheaper valuation).

**Verdict: Replacement approved.** Full Replacement Index workflow proceeds.

---

## Sleeve B Entry Reminder

Replacement Index approval does not override Sleeve B entry rules:

- New names require Investment Committee approval and LOS ≥ 80.
- Replacement is size-neutral: one-out-one-in at the same dollar value.
- Do not exceed `state.yaml: sleeves.b.ceiling_pct`.

The v5 method has no staged-entry sequence.

---

## LOS as a Complementary Layer

In LIOS v1.4+, **LOS** (LIOS Overall Score) is an **additional**
evaluation layer alongside the Replacement Index. The two are
complementary, not substitutes:

| Framework | What it measures |
|---|---|
| **Replacement Index** (method; Sleeve B under P6′) | Theme fit, thesis momentum, diversification value — surgical fit for Sleeve B |
| **LOS** (method) | Overall investment quality (value / growth / quality / momentum) — universal company quality |

**Joint workflow for replacement:**

1. Candidate must first clear LOS gate:
   - LOS ≥ 80 required for a new Sleeve B name
   - LOS < 70 → automatic Pass; do not advance
2. Then Replacement Index approval (this document):
   - Composite superiority over incumbent
   - ≥ 2 of 4 qualitative conditions met
3. Then confirm the Sleeve B ceiling and size-neutral replacement.

A candidate with high Frontier Score but weak LOS (e.g., LOS 65)
should **not** replace an incumbent without explicit IC rationale —
documented and disclosed in the Health Check.

See `core/research-layer.md` for the LOS framework.
See `core/sleeves.md` for Sleeve B entry and ceiling rules.

---

## Related
- See `core/sleeves.md` — Sleeve B rules
- See `constitution/method.md` — scoring and Replacement Index criteria
- See `core/research-layer.md` — LOS framework
- See `commands/los-replace.md` — LOS and Replacement Index workflow
