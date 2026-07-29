# `/los-compare` — Side-by-Side LOS Comparison

## Purpose

Compare multiple companies on the same five-score dimensions in a
single table. Surfaces relative attractiveness quickly.

## When to use

- Picking among candidates for a Sleeve B addition
- Deciding between a Sleeve D incumbent and a replacement candidate
- Validating that a new idea is genuinely better than what we hold

## Inputs required

1. List of 2-5 tickers or company names
2. Optional context: what decision is being informed

## Command

---

```
Compare the following companies using the LIOS Research Intelligence
Layer. Same five-score framework for each.

Companies: <TICKER1, TICKER2, TICKER3, ...>
Context (optional): <what decision this informs>

Output a comparison table:

| Ticker | LVS | LGS | LQS | LMS | LOS | Band | Recommendation | One-line Verdict |
|--------|----:|----:|----:|----:|----:|------|----------------|------------------|

Then a brief prose comparison:
- Top scorer and why
- Where each name excels / struggles
- Cross-name observations (e.g., "TICKER A has best LVS but
  weakest LMS — momentum laggard")
- Recommended pick if forced to choose (with Confidence %)

Rules:
- Same scoring rigor as /los. Same one-line rationales per score.
- Default LOS weighting: LQS 35 / LGS 25 / LVS 25 / LMS 15.
  Disclose any deviation.
- No hiding scores behind prose — table first, then analysis.

Begin.
```

---

## What you get back

Comparison table + prose verdict.

## Example output

| Ticker | LVS | LGS | LQS | LMS | LOS | Band | Rec | One-line |
|---|---:|---:|---:|---:|---:|---|---|---|
| EXAMPLE-B1 | 60 | 95 | 92 | 88 | 84 | Buy | BUY | Premium quality + growth; valuation is the friction |
| EXAMPLE-B2 | 78 | 90 | 85 | 75 | 87 | Buy | BUY | Quality compounder, cheap multiple |
| EXAMPLE-B3 | 65 | 88 | 90 | 82 | 85 | Buy | BUY | Quality + cash flow; cyclical exposure caps LOS |

**Top LOS**: EXAMPLE-B2 at 87. **Top LQS**: EXAMPLE-B3 at 90. **Top LGS**: EXAMPLE-B1 at 95.

**Recommended pick**: EXAMPLE-B2. Best risk-adjusted profile given current valuation.
Confidence: 72%.

## Discipline reminders

- **Same rigor per ticker.** Don't score one loosely and another tightly.
- **Recommendation must match band** unless IC override documented.
- **Don't hide the weakest.** If one name is clearly inferior, say so.

## Related

- See `los.md` — single-company scoring
- See `los-replace.md` — when comparing incumbent vs candidate for Sleeve D
- See `los-sleeve-b.md` — Sleeve B LOS review