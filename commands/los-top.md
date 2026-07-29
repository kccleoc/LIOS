# `/los-top` — Highest LOS in Current Holdings

## Purpose

Quickly surface the highest-scoring holdings by LOS in the current
portfolio. Useful for conviction checks and conviction-relative
position sizing discussion.

## When to use

- Before adding to an existing holding (verify it remains top-tier)
- When considering where to deploy incremental capital
- Quick situational read on portfolio quality distribution

## Inputs required

Live portfolio snapshot with current LOS values per holding.

If LOS values are not yet stored, run `portfolio-los.md` first.

## Command

---

```
Rank current portfolio holdings by LOS (highest first) using LIOS.

Snapshot + LOS data:
<paste snapshot with LOS per holding>

Output:

| Rank | Ticker | Sleeve | LOS | Band | Recommendation | MV | % NLV |
|------|--------|--------|----:|------|----------------|---:|------:|

Then a brief verdict:
- Top 3 names and why they're top
- Any names in Hold (60-69) band — flag
- Any names in Pass (<60) band — flag as replacement candidates
- Cross-sleeve observations

Rules:
- Sleeve A vehicles (from `state.yaml: positions` filtered to sleeve: A) excluded — LOS not applicable.
- Show MV and % NLV so conviction can be weighed against position size.
- Confidence % on the ranking.

Begin.
```

---

## What you get back

A ranked table of all scored holdings + conviction commentary.

## Example output

| Rank | Ticker | Sleeve | LOS | Band | Rec | MV | % NLV |
|------|--------|--------|----:|------|-----|---:|------:|
| 1 | EXAMPLE-B1 | B | 94 | Elite | Hold | $180,000 | 18.0% |
| 2 | EXAMPLE-B3 | B | 91 | Elite | Buy | $100,000 | 12.0% |
| 3 | EXAMPLE-D2 | D | 88 | Buy | Hold | $12,000 | 1.2% |
| 4 | EXAMPLE-B2 | B | 87 | Buy | Buy | $100,000 | 17.0% |
| 5 | EXAMPLE-D1 | D | 82 | Buy | Hold | $12,000 | 1.2% |
| ... | ... | ... | ... | ... | ... | ... | ... |

**Verdict**: Top tier (EXAMPLE-B1 / EXAMPLE-B3 / EXAMPLE-D2) drives portfolio quality.
Sleeve B well-positioned. No Pass-band holdings currently.

Confidence: 80%.

## Discipline reminders

- **Sleeve A vehicles are excluded** — LOS doesn't apply to broad indices.
- **Position size vs conviction**: a high-LOS holding with small MV is a different signal from a high-LOS holding at cap.

## Related

- See `portfolio-los.md` — full dashboard
- See `los-sleeve-b.md` — Sleeve B specific
- See `los-sleeve-d.md` — Sleeve D specific (LOS + Frontier)