# `/los` — Score a Single Company

## Purpose

Generate the full LOS research output (LVS / LGS / LQS / LMS / LOS)
for a single company using the standard format.

## When to use

- Researching a new candidate before Sleeve A/B/D consideration
- Re-scoring an existing holding after earnings or material change
- Any time Lc wants the structured research profile of a name

## Inputs required

1. Ticker (or company name)
2. Optional context: why Lc is researching it, what specifically to address
3. Live portfolio snapshot (only if scoring relative to existing holdings)

## Command

---

```
Score the following company using the LIOS Research Intelligence
Layer.

Company: <TICKER or NAME>
Context (optional): <why researching, specific concerns>

Output the standard research block:

Company: <TICKER>
LOS:    <NN>
LVS:    <NN>  — <one-line rationale citing specific data>
LGS:    <NN>  — <one-line rationale citing specific data>
LQS:    <NN>  — <one-line rationale citing specific data>
LMS:    <NN>  — <one-line rationale citing specific data>
Recommendation: <BUY / HOLD / REDUCE / EXIT / PASS / WATCH>
Summary: <one sentence>
Confidence: <XX>%

Rules:
- All four component scores (LVS/LGS/LQS/LMS) must have a one-line
  rationale citing specific data (no "looks cheap" — say "Forward
  P/E 14x vs 5-yr avg 22x").
- LOS weighting: default LQS 35 / LGS 25 / LVS 25 / LMS 15. If you
  deviate, disclose weights and rationale.
- Recommendation must match LOS band:
    90-100 = Elite (BUY / accumulate aggressively)
    80-89  = Buy (BUY / DCA eligible)
    70-79  = Watch (WATCHLIST / buy on correction)
    60-69  = Hold (HOLD / monitor)
    <60    = Pass (PASS / replace if held)
- Confidence % required.
- Output the band label too (e.g., "Elite", "Buy").

Begin.
```

---

## What you get back

The standard LOS research block + a brief verdict.

## Example output

```
Company: TXN
LOS:    81 (Buy)
LVS:    82  — Forward P/E 18x vs 5-yr avg 22x; FCF yield ~5%
LGS:    72  — Modest revenue growth (~5%); expanding industrial/auto TAM
LQS:    95  — Best-in-class ROIC (~30%); fortress balance sheet; wide moat in analog
LMS:    64  — Range-bound trend; mixed estimate revisions
Recommendation: BUY
Summary: Excellent business quality with attractive valuation,
         moderate growth outlook, and improving momentum.
Confidence: 78%
```

## Discipline reminders

- **Standard format is mandatory** — no freeform alternative.
- **Cite specific data** in every rationale. Generic descriptions are rejected.
- **Recommendation must match band** unless IC override is documented.

## Related

- See `../lios/components/08_research_intelligence_layer.md` — full LOS spec
- See `los-compare.md` — for side-by-side scoring of multiple names
- See `los-candidates.md` — candidate research workflow
- See `portfolio-los.md` — full portfolio dashboard