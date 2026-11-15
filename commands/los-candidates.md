> **v5 conformance note.** Sleeves are **A/B/C/Z** measured against NLV. There
> is no Sleeve D, no Kelly Factor, no Strategic Reserve and no `ABC` denominator.
> Where this file and `lios/v4.1.md` disagree, the init prompt wins.


# `/los-candidates` — Candidate Research Workflow

## Purpose

Walk one or more new idea(s) through the standard LIOS candidate
research workflow: Discover → LOS → Watchlist / Deep Research →
Deployment → Execution.

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
  - Sleeve fit: A / B / C / Z?
  - Funding source: does it respect the funding-source matrix?
  - If the candidate is an ETF, it belongs in A — state the written view statement
    (intended exposure, horizon, invalidation condition).
  - If it is a company, it belongs in B — state the LOS, the fit case, and whether
    B has ceiling headroom (B is cap-only and never receives Top-up).
  - Catalyst: what's the entry trigger?

Step 3: For LOS 80+ candidates passing Step 2:
  - Size is NOT set by a score. The Regime Index has no mechanical effect (P2′).
  - An ETF candidate enters Sleeve A under its written view statement; within-A
    sizing respects the thematic cap.
  - A company candidate enters Sleeve B only with IC approval and ceiling
    headroom. B has no target, so there is no size to compute from a gap.
  - Recommend: "Hold for IC review"

Output format:

| Candidate | LOS | Band | Workflow Step | Action | Confidence |
|-----------|----:|------|---------------|--------|------------|

Then a brief prose verdict per candidate.

Rules:
- LOS < 70 candidates are NOT pursued (no exceptions)
- LOS 70-79 go to watchlist with monitoring conditions
- LOS 80+ require explicit funding-source + sleeve-fit verification
- Sleeve B candidates at LOS 80+ still need IC approval and Replacement Index fit before entry

Begin.
```

---

## What you get back

Per-candidate workflow status + next-step directives.

## Example output

| Candidate | LOS | Band | Workflow Step | Action | Confidence |
|---|---:|---|---|---|---|
| `<TICKER>` | 85 | Buy | Deep Research | Sleeve B fit; hold for IC review | 75% |
| EXAMPLE-T1 | 58 | Pass | STOP | Do not pursue | 90% |
| EXAMPLE-T3 | 78 | Watch | Watchlist | Add to los-watchlist | 70% |
| `<TICKER>` | 82 | Buy | Deep Research | B candidate; needs IC approval and RI fit | 65% |

## Discipline reminders

- **LOS < 70 = no pursuit.** Period. Watchlist = 70-79 only.
- **LOS ≥ 80 is necessary but not sufficient** — funding source + sleeve fit + RI (if D) still apply.
- **Watchlist is not a buy queue.** It's a monitor queue with entry conditions.

## Related

- See `los.md` — single-company scoring
- See `los-watchlist.md` — watchlist management
- See `core/deployment.md` — Top-up and Rebalance (there is no sizing score)
- See `los-sleeve-b.md` — Sleeve B fit check
- See `../lios/components/02_capital_architecture.md` — funding sources