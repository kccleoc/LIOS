> **Source:** Governs Sleeve B. Moved from the v4.1 satellite sleeve (old C/D) to B in
> v5. Size-neutral replacement is a v5 rule.

# 04 · Sleeve B — Replacement Index

**Component version:** v6
**Status:** Method (`constitution/method.md`) / dynamic scores
**Last updated:** 2026-11-15

---

## Purpose

The Replacement Index is the **only** legitimate mechanism for Sleeve B
replacement. It exists to:

1. Prevent emotional / narrative-driven decisions
2. Force every B replacement to clear an objective bar
3. Make "should we replace this name?" answerable from data, not vibes
4. Keep Sleeve B inside its ceiling (`state.yaml: sleeves.b.ceiling_pct`)

It does not size positions, fund new ones, or raise total risky weight. B is
cap-only (P2′) and receives no Top-up (P6′). The index answers one question: *is the
candidate better than the incumbent?*

---

## The Three Metrics

Each metric is scored 0–100 for both the **incumbent** (the B name under review) and
the **candidate** (the proposed replacement).

| Metric | Weight | What it measures |
|---|---|---|
| **Frontier Score** | 40% | How well-positioned is this name within the frontier theme? TAM capture, technological edge, moat trajectory |
| **Thesis Momentum** | 35% | Is the thesis *strengthening or weakening*? Earnings trajectory, guidance, competitive position, catalysts |
| **Diversification Value** | 25% | How much does this name *add* to the existing B book? Correlation to other B names, theme coverage |

Higher score = stronger on that metric. The composite is weighted.

---

## Composite Score

```
Replacement_Index = 0.40*frontier + 0.35*momentum + 0.25*diversification
```

Range 0–100. Used to rank all B names and every candidate.

---

## Replacement Approval Criteria

A replacement is approved **only when both of the following are true**:

1. **Candidate composite > incumbent composite**, with documented deltas.
2. **At least two of these four conditions are also met:**
   - Incumbent misses earnings OR issues weak guidance
   - Incumbent thesis weakens materially (product, market position, regulation)
   - Candidate trades at a substantially more attractive valuation (for example
     > 20% cheaper on forward P/E or P/S)
   - Candidate theme conviction becomes significantly higher (breakthrough, major
     contract win)

If criterion 1 holds but fewer than two of (2) hold → **hold and re-review next
quarter.**
If two or more of (2) hold but criterion 1 fails → **do not replace**; the incumbent
still scores higher.

---

## Replacement Is Size-Neutral

Replacement is **one out, one in, same dollars**. The proceeds of the exit fund the
candidate; no new capital is introduced and no sleeve target moves.

Three consequences:

- **Total risky weight does not change.** `(A + B) / NLV` is identical before and
  after (P2′).
- **The B ceiling is not a funding source.** A replacement cannot push B above
  `sleeves.b.ceiling_pct`. If B is at the ceiling, the replacement is still
  size-neutral and the ceiling is unaffected.
- **A→B is a different operation.** Adding to B from A is gap-neutral and requires
  an Investment Committee decision (P4′); it is not a replacement and is not scored
  here as one.

Replacing a direct single-name holding with a fund carrying the same exposure is
**not** an exit and not a replacement (P5′) — the factor weight is unchanged and only
the wrapper changes.

---

## Anti-Patterns (will be rejected)

| Trigger | Why it is not a valid replacement rationale |
|---|---|
| Short-term price movement alone | Use the index, not candles |
| Unrealized loss on the incumbent | Sunk cost. Replacement is about forward expected value. |
| Excitement about a new name | Excitement is not a metric. Score it. |
| "It has been a while since we rotated" | Time in portfolio is not a metric. |
| One good earnings print on the candidate | One quarter is not a Thesis Momentum improvement. |
| "Replace because the incumbent fell" | A price fall is never an exit trigger (P5′) |
| "Fund the replacement with this month's contribution" | B is never topped up (P6′) |

---

## Scores Are Dynamic

A score is an **assessment, not a permanent label**. Re-review after:

- Quarterly earnings (incumbent or candidate)
- Material guidance changes
- Price moves greater than 20% in either direction
- Technology breakthroughs or major competitive shifts
- M&A events (acquirer or target)

If an incumbent's composite falls materially with no candidate in play, that is
itself a signal: flag it in the Sleeve B Review (§6 of the Health Check) and do not
auto-replace.

---

## Worked Example

Suppose Sleeve B holds **`<INCUMBENT>`** and a candidate **`<CANDIDATE>`** is
proposed.

| Metric | `<INCUMBENT>` | `<CANDIDATE>` |
|---|---|---|
| Frontier Score | 55 | 70 |
| Thesis Momentum | 40 | 65 |
| Diversification Value | 50 | 60 |
| **Composite** | **47.5** | **66.25** |

Criterion 1: PASS — candidate composite is higher.

Conditions:
- PASS — `<INCUMBENT>` missed last quarter's earnings
- FAIL — thesis on `<INCUMBENT>` weakened only modestly
- PASS — `<CANDIDATE>` trades roughly 30% cheaper on forward earnings
- FAIL — theme conviction roughly equal

Criterion 2: PASS — two conditions met.

**Verdict: Replacement approved**, size-neutral. The incumbent's dollars move to the
candidate, the B ceiling is unchanged, and total risky weight is unchanged.

---

## Entry Rules Still Apply

Replacement Index approval does not override Sleeve B entry rules:

- New names require Investment Committee approval and LOS ≥ 80.
- Replacement is size-neutral: one out, one in, at the same dollar value.
- `state.yaml: sleeves.b.ceiling_pct` is never exceeded by a replacement.
- Position-level exposure stays inside `sleeves.b.position_cap_pct`.

v5 has no staged-entry sequence. Staging belonged to the old satellite sleeve and is
deleted; B is governed by its ceiling, its position cap and the LOS gate.

---

## LOS as a Complementary Layer

LOS (LIOS Overall Score) is an **additional** layer alongside the Replacement Index.
The two are complementary, not substitutes:

| Framework | What it measures |
|---|---|
| **Replacement Index** | Theme fit, thesis momentum, diversification value — surgical fit for Sleeve B |
| **LOS** | Overall investment quality (value / growth / quality / momentum) — universal company quality |

**Joint workflow for a replacement:**

1. Candidate clears the LOS gate: LOS ≥ 80 for a new Sleeve B name; LOS < 70 is an
   automatic Pass and does not advance.
2. Replacement Index approval: composite superiority over the incumbent plus at least
   two of the four qualitative conditions.
3. Confirm the B ceiling and execute size-neutral.

A candidate with a high Frontier Score but a weak LOS should **not** replace an
incumbent without explicit IC rationale, documented and disclosed in the Health
Check.

See `08_research_intelligence_layer.md` for the LOS specification.

---

## Related

- `02_capital_architecture.md` — Sleeve B ceiling, funding, exit rules
- `core/replacement-index.md` — the method source for this component
- `08_research_intelligence_layer.md` — LOS framework
- `06_health_check_structure.md` §6 and §11 — B Review and Satellite Scoreboard
- `commands/los-replace.md` — the joint workflow command
