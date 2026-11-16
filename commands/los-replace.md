> **v4.1 conformance note.** Written for LIOS v2.x. In v4.1 the sleeves are
> **v6 conformance note.** Sleeves are **A/B/C** measured against NLV. The
> Replacement Index governs **Sleeve B** (satellites). Replacements are
> **size-neutral**: one out, one in, same dollars, so B's weight and the ceiling
> headroom do not change. Where this file and `lios/v4.1.md` disagree, the init
> prompt wins.


# `/los-replace` — Sleeve B Replacement (LOS + RI Joint Workflow)

## Purpose

Evaluate a Sleeve B replacement using both the LOS framework AND
the Replacement Index. This is the official LIOS workflow for any
Sleeve B surgery decision.

## When to use

- a Sleeve B incumbent is the weakest by Replacement Index
- After any D holding earnings or thesis event surfaces weakness
- At the quarterly Rebalance review

## Inputs required

1. Current Sleeve B holding to evaluate (incumbent)
2. Proposed replacement (candidate)
3. Live portfolio snapshot

## Command

---

```
Evaluate a Sleeve B replacement under LIOS (joint LOS + RI).

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

Gate 3 — Size neutrality + Sleeve B ceiling:
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
- **Size neutrality is non-negotiable.** A replacement is one-out-one-in at the same dollar size. It does not consume Sleeve B ceiling headroom and needs no funding from A.
- **Staging is not bypassed.** Even approved replacements follow Stage 1 → 2 → 3.

## Related

- See `../lios/components/04_replacement_index.md` — RI framework + LOS joint section
- See `../lios/components/08_research_intelligence_layer.md` — LOS framework
- See `sleeve-d-review.md` — RI-only review (broader sweep)
- See `los-sleeve-d.md` — joint review of all D holdings