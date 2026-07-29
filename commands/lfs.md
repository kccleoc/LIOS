# `/lfs` — Score a Single Company's LFS

## Purpose

Generate the full LFS research output for a single company using the five LFS components. LFS measures portfolio fit — how much this investment improves the current LIOS portfolio.

## When to use

- After a company passes LOS threshold (LOS ≥ 70), before capital allocation
- Re-scoring an existing holding after portfolio composition changes
- Comparing portfolio fit of a candidate against current holdings

## Inputs required

1. Ticker (or company name)
2. Live portfolio snapshot — LFS is relative, cannot be scored without knowing current portfolio
3. Optional context: why Lc is researching it, what specifically to evaluate

## Command

---

```
Score the following company using the LIOS Portfolio Construction
Intelligence layer (v1.5). Generate the full LFS (LIOS Fit Score)
research block.

Company: <TICKER or NAME>
Context (optional): <why researching, specific concerns>

Current Portfolio Snapshot:
<paste snapshot>

Output the LFS research block:

Company: <TICKER>
LOS:    <NN>  — <band> (include LOS for Decision Matrix evaluation)
LFS:    <NN>  — <band>
  Diversification Value: <NN> — <rationale: what economic engine does this add?>
  Strategic Fit:         <NN> — <rationale: how does this strengthen LIOS mission?>
  Overlap:               <NN> — <rationale: how much similar exposure exists?>
  Conviction Compatibility: <NN> — <rationale: can LIOS understand this business?>
  Portfolio Balance:     <NN> — <rationale: how does this improve construction?>
Decision Matrix: <LOS band> + <LFS band> → <Highest priority / Lower priority / Do not buy / Reject>
Recommendation: <BUY / HOLD / REDUCE / EXIT / PASS / WATCH>
Summary: <one sentence>
Confidence: <XX>%

Rules:
- LFS must be scored against the CURRENT portfolio snapshot provided.
- Each component score needs a one-line rationale citing specific analysis.
- LFS Scale:
    90-100 = Exceptional — adds meaningful diversification without sacrificing returns
    80-89  = Strong Fit — improves balance while maintaining quality
    70-79  = Useful — adds some diversification but is not essential
    60-69  = Neutral — does not materially improve the portfolio
    <60    = Poor Fit — adds unnecessary overlap or weak strategic value
- Include the LOS score (even if estimated) so the Decision Matrix is complete.
- Decision Matrix:
    High LOS + High LFS = Highest priority for new capital
    High LOS + Low LFS = Excellent company, lower portfolio priority
    Low LOS + High LFS = Do not buy (diversification alone insufficient)
    Low LOS + Low LFS = Reject
- If LOS is < 70, state that the candidate does not advance to LFS (L17 rule)
  and output "N/A — LOS below threshold. Do not pursue."
- Confidence % required on the LFS assessment.

Begin.
```

---

## What you get back

The full LFS research block + Decision Matrix verdict.

## Example output

```
Company: TXN
LOS:    84 (Buy)
LFS:    92 (Exceptional)
  Diversification Value: 95 — Adds analog/mixed-signal semiconductor engine; no overlap with existing digital AI compute names
  Strategic Fit:         88 — Supports durable compounding with best-in-class ROIC; strengthens Sleeve B philosophy
  Overlap:               90 — No current holding in analog semiconductor space; none of the existing 12 positions compete
  Conviction Compatibility: 92 — Simple business model; LIOS can monitor via book-to-bill, automotive/industrial end-market data
  Portfolio Balance:     95 — Reduces AI-centricity; adds industrial/automotive cyclical offset; complementary earnings driver
Decision Matrix: Buy + Exceptional → Highest priority for new capital
Recommendation: BUY
Summary: Exceptional portfolio addition — high quality, new economic engine, zero overlap.
Confidence: 85%
```

## Discipline reminders

- **LFS must always reference the live portfolio.** Same company, different portfolio = different LFS.
- **LOS comes first** — if LOS < 70, do not generate LFS (just report "below threshold").
- **Economic Engine Principle** — diversification is about economic drivers, not sector labels.

## Related

- See `../lios/components/09_portfolio_construction_intelligence.md` — full LFS spec
- See `los-lfs.md` — combined LOS + LFS evaluation
- See `portfolio-fit.md` — full portfolio fit analysis
- See `los.md` — single-company LOS scoring
