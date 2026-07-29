# `/sr-rank` — Strategic Reserve Deployment Ranking

## Purpose

If a Trigger Check confirms SR deployment is on the table, rank
the deployment opportunities so the IC can decide.

**Guides but does NOT auto-trigger purchases.**

## When to use

- **Only after** `/trigger-check` confirms ≥1 trigger is firing
- Before any IC SR deployment decision
- When Lc has new external capital to inject and wants a ranking

## Inputs required

1. Live portfolio snapshot
2. Current SR balance
3. Any context from the trigger (which trigger fired, why)
4. Optional: any candidate names Lc is already considering

## Command

---

```
Rank SR deployment opportunities under LIOS.

SR Balance: $X,XXX
Trigger context: <which trigger(s) fired, brief rationale>

Output a ranked table with these columns:

| Rank | Asset | Expected Return | Kelly Edge | Valuation | Diversification Benefit | Action |

For each candidate asset (sleeves, holdings, or new names):
- Expected Return: forward-looking, with rationale
- Kelly Edge: does this deployment improve or worsen the Kelly Factor?
- Valuation: cheap / fair / expensive — relative to own history + market
- Diversification Benefit: does this ADD to portfolio or concentrate further?
- Action: concrete directive — Deploy $X / Skip / Hold for next cycle

Deployment hierarchy (mandatory order — do not skip ranks):
1. Existing high-conviction Sleeve B positions
2. Sleeve D expansion (must still satisfy D staging rules)
3. New approved tech waves
4. Sleeve A on broad valuation opportunity

Rules:
- The hierarchy is fixed. If a rank-1 opportunity is unavailable,
  proceed to rank-2, etc.
- Each rank must show the rationale for its placement.
- Dollar amounts always.
- Confidence % per opportunity.
- Final recommendation: "Deploy $X of $Y total SR" — explicit.

Begin.
```

---

## What you get back

A ranked deployment table + explicit total recommendation.

## Example output shape

| Rank | Asset | Expected Return | Kelly Edge | Valuation | Diversification Benefit | Action |
|---|---|---|---|---|---|---|---|
| 1 | EXAMPLE-B1 add | 18-25% NTM | Improves | Fair | Concentrates (already large) | Deploy $2,000 |
| 2 | EXAMPLE-B2 add | 22-30% NTM | Improves | Cheap | Diversifies (memory exposure) | Deploy $1,500 |
| 3 | EXAMPLE-B3 add | 15-20% NTM | Neutral | Fair | Diversifies | Hold for next cycle |
| 4 | Sleeve D expansion (EXAMPLE-D1 Stage 3 top-up) | 20-30% NTM | Improves | Fair | Concentrates in cyber | Deploy $1,000 |
| 5 | Sleeve A rebalance (none warranted) | n/a | n/a | n/a | n/a | Skip |

**Recommendation**: Deploy $4,500 of $15,000 SR. Confidence: 75%.

## Discipline reminders

- **IC has sole deployment authority.** This ranking is a guide.
- **Deployment hierarchy is fixed.** Don't deploy into rank-3 before exhausting rank-1.
- **D staging still applies.** SR doesn't bypass Stage 2/3 gates.
- **No deployment into `state.yaml: sleeve_a.permanently_exited`.** (Your permanently-exited list is binding — was L7; portfolio-local discipline since v3.0.)
- **No deployment that violates funding-source matrix.** SR funds specific things; verify compliance.

## Related

- See `commands/trigger-check.md` — must run first
- See `../lios/components/02_capital_architecture.md` Strategic Reserve — funding/deployment rules
- See `../lios/components/06_health_check_structure.md` §10 — SR Opportunity Ranking in Health Check