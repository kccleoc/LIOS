# `/los-sleeve-b` — Sleeve B LOS Review

## Purpose

Review LOS for all Sleeve B holdings in a single pass. Identifies
which Sleeve B names still have Elite/Buy-grade quality, and which
have slipped into Hold/Pass territory.

## When to use

- Monthly Sleeve B check
- After any Sleeve B earnings
- Before adding to or trimming a Sleeve B position

## Inputs required

Live portfolio snapshot with current LOS per Sleeve B holding.

## Command

---

```
Review Sleeve B holdings using the LIOS Research Intelligence Layer.

Sleeve B holdings (from snapshot):
<paste Sleeve B rows>

Output a per-holding table:

| Ticker | Shares | MV | % NLV | Cap | Cap Headroom | LOS | Band | Rec | Action |
|--------|-------:|---:|------:|----:|-------------:|----:|------|-----|--------|

Then a verdict block:
- Each Sleeve B name: still high conviction (LOS ≥ 80), drifting
  (70-79), or in trouble (<70)?
- Any name with LOS < 70 → flag as Pass; existing holder monitor;
  trim candidate per Replacement Index workflow if applicable
- Any name at or near cap → restate buying-pause / trim signal
- Recommended action per holding with Confidence %

Rules:
- All four component scores must be shown for completeness (LVS, LGS, LQS, LMS)
- Cap headroom = (Cap $) - (Current MV)
- Sleeve B addition typically requires LOS ≥ 80 (per IC discipline)
- Cap rules (L3) are binding — do NOT recommend buying past cap
- If a Sleeve B holding has LOS < 70 AND cap headroom exhausted,
  flag for trim/exit consideration

Begin.
```

---

## What you get back

Per-name Sleeve B LOS review with cap-relative action recommendations.

## Example output

| Ticker | Shares | MV | % NLV | Cap | Headroom | LOS | Band | Rec | Action |
|---|---:|---:|---:|---:|---:|---:|---|---|---|
| EXAMPLE-B1 | 1,000.00 | $180,000 | 18.00% | 20% | $20,000 | 94 | Elite | Hold | No add (approaching cap); trim if exceeded |
| EXAMPLE-B2 | 200.00 | $100,000 | 17.00% | 5% | −$95,000 | 87 | Buy | **Trim** | Currently over 5% cap — trim to cap |
| EXAMPLE-B3 | 400.00 | $100,000 | 12.00% | 6% | −$40,000 | 91 | Elite | **Trim** | Currently over 6% cap — trim to cap |

**Verdict**: EXAMPLE-B1 near cap — discipline test. EXAMPLE-B2 and EXAMPLE-B3 over cap — trim recommended. Re-add only when within `state.yaml: sleeve_b_caps`.

Confidence: 82%.

## Discipline reminders

- **Caps are binding** (L3) — don't recommend buys past cap.
- **LOS < 70 in Sleeve B** → flag, even if cap not hit. Watch for slide to Pass band.
- **Cap hits = buying pause + trim signal**, not "hold forever."
- **Sleeve B additions need IC approval + LOS ≥ 80** as a working discipline.

## Related

- See `los.md` — single-company scoring
- See `los-top.md` — highest LOS across portfolio
- See `../lios/components/02_capital_architecture.md` Sleeve B