# `/los-watchlist` — Manage the LOS Watchlist

## Purpose

Maintain a watchlist of names that have cleared the LOS 70+
threshold but are not yet held at Stage 2 (Pilot) or above. This is
the structured pre-deployment queue.

## When to use

- After running `/los` on candidates that scored 70-79
- Quarterly watchlist review
- Before considering Sleeve D additions

## Inputs required

1. List of candidate names Lc is considering for the watchlist
2. Existing watchlist (to be preserved/updated)

## Command

---

```
Manage the LIOS watchlist using the Research Intelligence Layer.

New candidates to evaluate: <list or "none">
Existing watchlist: <paste current list or "empty">

For each new candidate: run the standard /los scoring format.
Skip any that score below 70 (they don't enter the watchlist).

Output:

| Ticker | LOS | Band | Date Added | Theme / Note | Next Action |
|--------|----:|------|------------|--------------|-------------|
| ... | ... | ... | YYYY-MM-DD | ... | ... |

Then a brief watchlist summary:
- Total names on watchlist
- Distribution by band (Watch 70-79 vs Buy 80-89)
- Names that have been on the watchlist longest without action
  (candidates for stale-watchlist review — drop or re-evaluate)
- Any names ready to advance to deep research / Kelly allocation

Rules:
- Watchlist threshold: LOS ≥ 70 (Locked Decision L11/L12)
- Sleeve D candidates typically need LOS ≥ 75 to advance
- Sleeve B additions typically need LOS ≥ 80
- Stale names (>90 days without action or re-evaluation) should be
  flagged for re-score or removal

Begin.
```

---

## What you get back

Watchlist table + management summary.

## Example output

| Ticker | LOS | Band | Date Added | Theme | Next Action |
|---|---:|---|---|---|---|
| TSLA | 58 | Pass | 2026-06-10 | EV / autonomy | ❌ Dropped (LOS < 70) |
| SNOW | 78 | Watch | 2026-07-15 | Data cloud | Monitor for correction |
| AVGO | 85 | Buy | 2026-07-22 | Semis / AI | Deep research → Kelly |
| CRDO | 72 | Watch | 2026-05-30 | Credit | Re-score after Q2 earnings |

**Summary**: 6 names active. 2 in Buy band (eligible for Kelly). 1 stale (>90 days).

## Discipline reminders

- **Watchlist threshold is LOS ≥ 70**, not "interesting."
- **Stage 2 (Pilot) requires Sleeve D staging rules** — LOS alone doesn't bypass them.
- **Re-score after earnings** — a 6-month-old LOS is stale.

## Related

- See `los-candidates.md` — full candidate research workflow
- See `../lios/components/02_capital_architecture.md` Sleeve D — staging rules
- See `portfolio-los.md` — current portfolio LOS context