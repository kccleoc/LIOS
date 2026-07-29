# `/los-candidates` — Candidate Research Workflow

## Purpose

Walk one or more new idea(s) through the standard LIOS candidate
research workflow: Discover → LOS → Watchlist / Deep Research →
Kelly → Execution.

## When to use

- Lc has a new ticker or theme to evaluate
- Quarterly "what new ideas are out there?" sweep
- Surfacing Sleeve B/D candidates systematically

## Inputs required

1. One or more candidate tickers / themes
2. Optional: any context (theme, sector, source of idea)

## Command

---

```
Run the LIOS candidate research workflow.

Candidates: <TICKER1, TICKER2, ...>
Context (optional): <theme / sector / source idea>

For each candidate, run the standard /los scoring format.

Then apply the gating workflow:

Step 1: LOS Gate
  - LOS < 70 → STOP. Do not advance. State rationale.
  - LOS 70-79 → WATCHLIST (add to los-watchlist)
  - LOS 80+ → DEEP RESEARCH

Step 2: For LOS 80+ candidates, additional checks:
  - Sleeve fit: A / B / C / D / SR?
  - Funding source: does it respect the funding-source matrix?
  - Staging (if Sleeve D): what stage would it enter?
  - Catalyst: what's the entry trigger?

Step 3: For LOS 80+ candidates passing Step 2:
  - Recommend: "Run /kelly-recalc for sizing decision"
  - Or: "Hold for IC review"

Output format:

| Candidate | LOS | Band | Workflow Step | Action | Confidence |
|-----------|----:|------|---------------|--------|------------|

Then a brief prose verdict per candidate.

Rules:
- LOS < 70 candidates are NOT pursued (no exceptions)
- LOS 70-79 go to watchlist with monitoring conditions
- LOS 80+ require explicit funding-source + sleeve-fit verification
- Sleeve D candidates at LOS 80+ still need RI approval before staging

Begin.
```

---

## What you get back

Per-candidate workflow status + next-step directives.

## Example output

| Candidate | LOS | Band | Workflow Step | Action | Confidence |
|---|---:|---|---|---|---|
| AVGO | 85 | Buy | Deep Research | Sleeve B fit; run /kelly-recalc | 75% |
| TSLA | 58 | Pass | STOP | Do not pursue | 90% |
| SNOW | 78 | Watch | Watchlist | Add to los-watchlist | 70% |
| XYZ (quantum) | 82 | Buy | Deep Research | Sleeve D Stage 1 candidate; needs RI check | 65% |

## Discipline reminders

- **LOS < 70 = no pursuit.** Period. Watchlist = 70-79 only.
- **LOS ≥ 80 is necessary but not sufficient** — funding source + sleeve fit + RI (if D) still apply.
- **Watchlist is not a buy queue.** It's a monitor queue with entry conditions.

## Related

- See `los.md` — single-company scoring
- See `los-watchlist.md` — watchlist management
- See `kelly-recalc.md` — sizing decision
- See `los-sleeve-d.md` — Sleeve D fit check
- See `../lios/components/02_capital_architecture.md` — funding sources