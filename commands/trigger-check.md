> **RETIRED in LIOS v4.1.** L6 (Strategic Reserve) is repealed. The trigger
> ladder is replaced by the rebalance band in `core/deployment.md` Rule 2.
> This file is retained for historical reference only.

# `/trigger-check` — SR Deployment Trigger Scan

## Purpose

Determine whether any Strategic Reserve deployment triggers are
currently firing. This is the gate before any SR capital can be
considered for deployment.

## When to use

- After major market moves
- After earnings shocks
- Before any IC SR decision
- Whenever Lc is considering deploying SR ("should I pull the trigger?")

## Inputs required

1. Live portfolio snapshot
2. Current market data (index levels, recent drawdowns)
3. Any specific thesis-conviction events Lc wants considered

## Command

---

```
Run a LIOS SR Trigger Check.

Snapshot:
<paste live snapshot here>

Scan each of the following triggers and answer yes/no with evidence:

1. S&P correction > 8-10% from recent peak?
   - Current vs recent peak
   - Days since correction began
   - Reason (if known)

2. Nasdaq correction > 10% from recent peak?
   - Current vs recent peak
   - Reason (if known)

3. High-conviction unjustified selloff?
   - Name(s) involved
   - Why "unjustified" — what thesis is intact?
   - Drawdown magnitude

4. Earnings confirm stronger thesis while price declines?
   - Name(s)
   - Specific guidance / margin / beat data
   - Price reaction

5. Kelly Factor increases meaningfully?
   - Previous vs current Kelly Factor
   - Which dimensions changed

6. IC formal approval requested?
   - Requester
   - Rationale
   - Proposed deployment size

For each: state yes/no, then provide evidence (cite the data).
If multiple triggers fire, rank by signal strength.

Rules:
- A trigger firing is necessary but NOT sufficient for SR deployment.
- Final deployment requires IC approval regardless.
- Do not invent triggers — only fire on evidence.
- Confidence % on each trigger assessment.

Begin.
```

---

## What you get back

A trigger matrix with yes/no + evidence, then a recommended next step:

- **No triggers firing** → "Hold SR. No action."
- **1 trigger firing** → "IC evaluation warranted. Run `/sr-rank` for opportunities."
- **Multiple triggers firing** → "Multiple signals align. Recommend IC convene to discuss SR deployment. Run `/sr-rank` immediately."

## Discipline reminder

A trigger firing does NOT auto-deploy. Triggers open the door to
deployment consideration; the IC decides whether to walk through.

The SR Opportunity Ranking (section 10 of Health Check) is a guide,
not an order. See `commands/sr-rank.md`.

## Related

- See `../lios/components/02_capital_architecture.md` Strategic Reserve — trigger definitions
- See `commands/sr-rank.md` — what to run if a trigger fires
- See `../lios/components/06_health_check_structure.md` §4 — Trigger Check as part of full Health Check