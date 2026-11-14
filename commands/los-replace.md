> **v3.0 conformance note.** Written for LIOS v2.x. In v3.0 the sleeves are
> **A/B/C/D/Z** measured against NLV, the Kelly Factor is replaced by the
> **Bull–Bear Index**, and the Strategic Reserve is repealed. Where this file
> and `lios/v3.0.md` disagree, v3.0 wins.


# `/los-replace` — Sleeve D Replacement (LOS + RI Joint Workflow)

## Purpose

Evaluate a Sleeve D replacement using both the LOS framework AND
the Replacement Index. This is the official LIOS workflow for any
Sleeve D surgery decision.

## When to use

- Lc is considering replacing a Sleeve D incumbent
- After any D holding earnings or thesis event surfaces weakness
- During quarterly Sleeve D review

## Inputs required

1. Current Sleeve D holding to evaluate (incumbent)
2. Proposed replacement (candidate)
3. Live portfolio snapshot

## Command

---

```
Evaluate a Sleeve D replacement under LIOS (joint LOS + RI).

Incumbent: <TICKER>
Candidate: <TICKER>
Context (optional): <earnings event, thesis shift, etc.>

Score BOTH names using both frameworks:

A. LOS Components (each 0-100):
   LVS: <NN> — <one-line rationale>
   LGS: <NN> — <one-line rationale>
   LQS: <NN> — <one-line rationale>
   LMS: <NN> — <one-line rationale>
   LOS:  <composite>  (band: Elite/Buy/Watch/Hold/Pass)

B. Replacement Index Components (each 0-100):
   Frontier Score (40%): <NN> — <one-line rationale>
   Thesis Momentum (35%): <NN> — <one-line rationale>
   Diversification Value (25%): <NN> — <one-line rationale>
   RI Composite: <composite>

Then evaluate the gates:

Gate 1 — LOS gate:
  - Candidate LOS ≥ 75 → eligible
  - Candidate LOS < 70 → automatic PASS, do not advance
  - Candidate LOS 70-74 → Watch; re-evaluate next cycle

Gate 2 — Replacement Index (L5):
  (a) Candidate RI composite > Incumbent RI composite
  (b) ≥ 2 of 4 qualitative conditions met:
      - Incumbent missed earnings / weak guidance
      - Incumbent thesis weakened materially
      - Candidate valuation substantially more attractive
      - Candidate wave significantly higher conviction

Gate 3 — Funding source + Sleeve D staging:
  - Funding source respects matrix (external capital only for D)
  - Candidate would enter Stage 1 / 2 / 3 per current rules

Output the verdict:
  - "Replace" if all three gates pass
  - "Hold" if any gate fails
  - "Defer" if borderline (state why + re-evaluation trigger)

Confidence: XX%

Begin.
```

---

## What you get back

Joint LOS + RI evaluation + replacement verdict.

## Example output

```
Incumbent: <INCUMBENT> (LOS 65, RI 47.5)
Candidate:  <CANDIDATE> (LOS 88, RI 66.25)

Gate 1 — LOS: ✅ Candidate LOS 88 ≥ 75. Eligible.
Gate 2 — RI:
  (a) ✅ 66.25 > 47.5
  (b) ✅✅❌❌
      ✅ Incumbent missed Q1 earnings
      ✅ Candidate trades at 30% cheaper forward P/E
      ❌ Thesis roughly equal
      ❌ Wave conviction roughly equal
  → 2 of 4 conditions met ✅

Gate 3 — Funding source: ✅ External capital used (not DCA / other sleeves)
        Staging: candidate would enter Stage 1 → 2 → 3

Verdict: REPLACE. Confidence: 80%.
```

## Discipline reminders

- **All three gates must pass.** LOS alone is insufficient.
- **No surgery on LOS < 75 candidate.** Even if incumbent is weaker.
- **Funding source compliance is non-negotiable.** Sleeve D capital has strict sources.
- **Staging is not bypassed.** Even approved replacements follow Stage 1 → 2 → 3.

## Related

- See `../lios/components/04_replacement_index.md` — RI framework + LOS joint section
- See `../lios/components/08_research_intelligence_layer.md` — LOS framework
- See `sleeve-d-review.md` — RI-only review (broader sweep)
- See `los-sleeve-d.md` — joint review of all D holdings