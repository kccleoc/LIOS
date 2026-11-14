# `/snapshot` — Generate a Dated Snapshot

## Purpose

Produce a point-in-time snapshot from `portfolio/state/portfolio.yaml`. No
analysis, no recommendations — a faithful render of state.

## When to use

- After any state update
- Before an Investment Committee decision
- At the end of a monthly cycle
- Before a specific event, so there is a "before" to diff against

## Inputs

1. `portfolio/state/portfolio.yaml` (never a pasted print, unless no state exists)
2. Price date

## Command

---

```
Generate a LIOS v3.0 portfolio snapshot from this state file.

State:
<paste state.yaml here>

Output the snapshot template with:
1. Account summary: NLV, gross position value, cash
2. Holdings: ticker, shares, price, market value, sleeve
3. Totals: gross position value, cash, NLV
4. Sleeve breakdown against targets:
     A core index | B conviction | C satellite | D duration | Z cash
   Show MV, % of NLV, target %, delta pp
5. Constraint snapshot:
     effective single-name exposure, factor, duration, total risky, US-situs
6. RI line: weighted value, band, date scored

Rules:
- Sleeves are A/B/C/D/Z. There is no SR, no ABC denominator
- Z is cash equivalents only (duration ≈ 0)
- All percentages are of NLV
- If a field is unavailable in state, write "not in state" — never estimate
- No recommendations. This is a render, not an opinion

Begin.
```

---

## What you get back

A dated file matching `templates/Snapshot.md`, suitable for
`portfolio/snapshots/YYYY-MM-DD_portfolio.md`.

## Related

- `../templates/Snapshot.md`
- `../core/sleeves.md`
- `../core/validation.md`
