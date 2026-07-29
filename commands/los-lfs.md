# `/los-lfs` — Combined LOS + LFS Evaluation

## Purpose

Generate both LOS (company quality) and LFS (portfolio fit) for a single company in one pass. Produces the full Decision Matrix output.

## When to use

- Full candidate evaluation — new idea, replacement candidate, or any name needing both scores
- Before capital allocation decisions for LOS-qualified candidates
- After portfolio composition changes that could affect LFS

## Inputs required

1. Ticker (or company name)
2. Live portfolio snapshot — required for LFS scoring
3. Optional context: what decision this informs

## Command

---

```
Evaluate the following company using the full LIOS v1.5 framework.
Generate BOTH the LOS (Research Intelligence Layer) and LFS (Portfolio
Construction Intelligence) research blocks.

Company: <TICKER or NAME>
Context (optional): <what decision this informs>

Current Portfolio Snapshot:
<paste snapshot>

Output both research blocks in order:

--- LOS ---
Company: <TICKER>
LOS:    <NN>  — <band>
LVS:    <NN>  — <one-line rationale>
LGS:    <NN>  — <one-line rationale>
LQS:    <NN>  — <one-line rationale>
LMS:    <NN>  — <one-line rationale>

--- LFS ---
LFS:    <NN>  — <band>
  Diversification Value: <NN> — <rationale>
  Strategic Fit:         <NN> — <rationale>
  Overlap:               <NN> — <rationale>
  Conviction Compatibility: <NN> — <rationale>
  Portfolio Balance:     <NN> — <rationale>

--- DECISION MATRIX ---
LOS Band: <Elite / Buy / Watch / Hold / Pass>
LFS Band: <Exceptional / Strong Fit / Useful / Neutral / Poor Fit>
Matrix Verdict: <Highest priority / Lower priority / Do not buy / Reject>

Recommendation: <BUY / HOLD / REDUCE / EXIT / PASS / WATCH>
Summary: <one sentence>
Confidence: <XX>%

Rules:
- LOS: Default LQS 35 / LGS 25 / LVS 25 / LMS 15. Disclose deviations.
- LFS: Must reference the live portfolio — same company, different portfolio = different LFS.
- Both sets of component scores need one-line rationales with specific data.
- If LOS < 70: Output LOS block, do NOT output LFS block. State: "LOS below threshold (LFS not evaluated — L17)."
- If LOS ≥ 70: Output both blocks and complete the Decision Matrix.
- Decision Matrix rules:
    High LOS + High LFS = Highest priority for new capital
    High LOS + Low LFS = Lower portfolio priority
    Low LOS + High LFS = Do not buy (diversification alone insufficient)
    Low LOS + Low LFS = Reject

Begin.
```

---

## What you get back

Both LOS and LFS research blocks + Decision Matrix verdict.

## Example output

```
--- LOS ---
Company: TXN
LOS:    84 (Buy)
LVS:    82 — Forward P/E 18x vs 5-yr avg 22x; FCF yield ~5%
LGS:    72 — Modest revenue growth (~5%); expanding industrial/auto TAM
LQS:    95 — Best-in-class ROIC (~30%); fortress balance sheet; wide moat in analog
LMS:    64 — Range-bound trend; mixed estimate revisions

--- LFS ---
LFS:    92 (Exceptional)
  Diversification Value: 95 — New analog engine; zero overlap with existing portfolio
  Strategic Fit:         88 — Durable compounding; fits Sleeve B philosophy
  Overlap:               90 — No overlapping positions in analog semiconductors
  Conviction Compatibility: 92 — Simple, transparent business model
  Portfolio Balance:     95 — Reduces AI concentration; adds industrial exposure

--- DECISION MATRIX ---
LOS Band: Buy | LFS Band: Exceptional
Matrix Verdict: Highest priority for new capital

Recommendation: BUY
Summary: Outstanding company that would significantly improve the portfolio.
Confidence: 82%
```

## Discipline reminders

- **LOS and LFS measure different things.** Do not average them or combine into a single score.
- **Portfolio snapshot is mandatory for LFS** — if none provided, generate LOS only.
- **If LOS and LFS conflict** (e.g., High LOS + Low LFS), explain the tension.

## Related

- See `los.md` — LOS-only scoring
- See `lfs.md` — LFS-only scoring
- See `../lios/components/09_portfolio_construction_intelligence.md` — Investment Decision Matrix
