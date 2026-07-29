# `/health-check` — Full 16-Section Report

## Purpose

Run the complete LIOS Health Check. This is the canonical full review
and should be the default when Lc wants a comprehensive read on the
portfolio.

## When to use

- Monthly review (always)
- After major market moves (>5% NLV swing)
- Before Investment Committee decisions on SR deployment
- Quarterly as a full reset

## Inputs required

1. **Live portfolio snapshot** (paste from `snapshots/`)
2. Current market data (or note that AI may use most recent available)
3. Any context Lc wants surfaced (specific concerns, open questions)

## Command

Paste the following into a LIOS-initialized session, after providing
the live snapshot:

---

```
Run a full LIOS Health Check on my portfolio.

Snapshot:
<paste live snapshot here>

Output the complete 16-section report in the canonical order:
1. Portfolio Health
2. Drift Check
3. Kelly Dashboard
4. Trigger Check
5. Sleeve A Review
6. Sleeve B Review
7. Sleeve C Review
8. Sleeve D Review
9. Strategic Reserve Review
10. Strategic Reserve Opportunity Ranking
11. Frontier Scoreboard
12. Technology Wave Map
13. Execution Calendar
14. Execution Plan
15. Risk Assessment
16. CIO Resolution

Rules:
- Conclusions first, reasoning second
- Exact dollar amounts always (never percentages alone)
- Explicit directives: Buy / Hold / Reduce / Exit
- Facts separated from Assessments
- Confidence % on every recommendation in sections 5, 6, 8, 14, 15
- No skipped sections — if no news, state "no action" with one-line rationale
- End with CIO Resolution as one paragraph

Begin.
```

---

## What you get back

A complete 16-section report following the spec in
`../lios/components/06_health_check_structure.md`. Section 14
(Execution Plan) is the actionable core — that's where the
buy/hold/reduce/exit directives live.

## Respect locked decisions

Every directive in section 14 must respect:
- Sleeve A targets (L2)
- Sleeve A targets (L2 form; values in `state.yaml: sleeve_a.allocation`)
- Sleeve B caps (L3 form; values in `state.yaml: sleeve_b_caps`)
- Kelly v2.0 framework (L4)
- Replacement Index for D (L5)
- SR funding/deployment rules (L6)
- `state.yaml: sleeve_a.permanently_exited` is binding for your portfolio (was L7; portfolio-local since v3.0)

If a directive would violate a Locked Decision, the report must
flag it explicitly and recommend either (a) the framework-compliant
alternative or (b) a constitutional amendment — never silently violate.