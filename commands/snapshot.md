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
Generate a LIOS v5 portfolio snapshot from this state file.

State:
<paste state.yaml here>

Output the snapshot template with:
1. Account summary: NLV, gross position value, cash
2. Holdings: ticker, shares, price, market value, sleeve
3. Totals: gross position value, cash, NLV
4. Sleeve breakdown against targets:
     A ETFs (core index + sector/thematic) | B satellites | C defensive (cash/bills/bonds)
   Show MV, % of NLV, target %, delta pp, band status
   B has a CEILING and no target: show its distance to the ceiling instead of a
   target and a delta
5. Constraint snapshot:
     look-through single-company exposure, single-sector exposure, duration,
     total risky = (A + B) / NLV, US-situs, B ceiling headroom
6. Replacement Index line: B holdings ranked, weakest named, date scored

Rules:
- Sleeves are A/B/C. There is no Sleeve D, no SR, no Kelly Reserve, no ABC denominator
- C is the defensive sleeve: cash, bills and bonds, with average duration 1–6 years and no single holding above 10
  (duration near zero). A cash-like instrument is never rendered inside A or C
- All percentages are of NLV. One denominator only
- B is cap-only. Do not render a target or a gap for B
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
