# `/los-sleeve-b` — Sleeve B LOS Review

## Purpose

Review LOS for all Sleeve B holdings in a single pass. Identifies
which Sleeve B names still have Elite/Buy-grade quality, and which
have slipped into Hold/Pass territory or into a thesis-exit review.

Sleeve B is **cap-only**: it has a ceiling and no target weight. There
is no equal-weight rule, no per-slot band, and no drift-to-target to
report. The only two questions are ceiling status and thesis status.

## When to use

- Monthly Sleeve B check
- After any Sleeve B earnings
- Before any Investment Committee decision to add to Sleeve B
- During the quarterly Rebalance review, when the B ceiling is tested

## Inputs required

Live portfolio snapshot with current LOS per Sleeve B holding.

## Command

---

```
Review Sleeve B holdings using the LIOS Research Intelligence Layer.

Sleeve B holdings (from snapshot):
<paste Sleeve B rows>

Output a per-holding table:

| Ticker | Shares | MV | % NLV | Position Cap | Headroom | LOS | Band | Rec | Action |
|--------|-------:|---:|------:|-------------:|---------:|----:|------|-----|--------|

Then a verdict block:
- Sleeve B total as % of NLV against the ceiling, and the dollar overage
  if any. Between quarterly reviews an overage is reported, not traded
- Each Sleeve B name: still high conviction (LOS >= 80), drifting
  (70-79), or in trouble (< 70)?
- Any name with LOS < 60 -> thesis-exit test: open a 30-day deep-dive
  review that resolves to a documented re-score of 60 or above, or exit.
  No third outcome
- Recommended action per holding with Confidence %

Rules:
- All four component scores must be shown for completeness (LVS, LGS, LQS, LMS)
- Headroom = (Position Cap x NLV) - Current MV
- Sleeve B has no target weight. Do NOT report drift versus a target,
  and do NOT propose equal weighting or per-slot bands
- Sleeve B receives no Top-up. Report $0.00 from this month's contribution
- Sleeve B additions require Investment Committee approval and LOS >= 80
- Cap rules (P6') are binding — do NOT recommend buying past cap or past
  the sleeve ceiling
- Trimming is risk control: at the quarterly review, trim the LARGEST
  position, never the weakest. Selection of the weakest is a Replacement
  Index judgment, not a trim rule
- A trim is not an exit. Price decline, drawdown and valuation are never
  exit triggers (P5')

Begin.
```

---

## What you get back

Per-name Sleeve B LOS review with ceiling-relative and cap-relative
action recommendations.

## Example output

| Ticker | % NLV | Position Cap | Headroom (pp) | LOS | Band | Rec | Action |
|---|---:|---:|---:|---:|---|---|---|
| `<TICKER-B1>` | 4.0% | 5.0% | 1.0 | 94 | Elite | Hold | No add — near cap |
| `<TICKER-B2>` | 6.0% | 5.0% | −1.0 | 87 | Buy | **Reduce** | Over position cap — trim to cap |
| `<TICKER-B3>` | 2.0% | 5.0% | 3.0 | 52 | Pass | **Review** | LOS < 60 — 30-day deep-dive open |

**Verdict**: `<TICKER-B1>` near cap — no add. `<TICKER-B2>` over its
position cap — trim to cap. `<TICKER-B3>` below the thesis-exit line —
30-day review is now open.

Confidence: 82%.

## Discipline reminders

- **B is cap-only.** A ceiling, not a target. No equal weight at entry, no
  per-slot band, no drift-to-target report.
- **B is never topped up.** The monthly contribution never touches it. Funding
  arrives only by Investment Committee decision from Sleeve A.
- **Caps are binding** (P6′) — don't recommend buys past the position cap or
  past the sleeve ceiling.
- **Trims take the largest position**, at the quarterly review. Trim = risk
  control, not an exit.
- **The only exit is a thesis break** (P5′). LOS < 60 opens a 30-day review;
  LOS 60–69 is a watch flag, not an exit.
- **Sleeve B additions need IC approval + LOS ≥ 80.**

## Related

- See `los.md` — single-company scoring
- See `los-top.md` — highest LOS across portfolio
- See `../core/sleeves.md` — Sleeve B rules
- See `../core/replacement-index.md` — replacement test for B
