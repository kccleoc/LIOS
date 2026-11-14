> **v4.1 conformance note.** Written for LIOS v2.x. In v4.1 the sleeves are
> **A/B/C/D/Z** measured against NLV, the Kelly Factor is replaced by the
> **Regime Index**, and the Strategic Reserve is repealed. Where this file
> and `lios/v4.1.md` disagree, v3.0 wins.


# `/health-check` — Full 16-Section Report

## Purpose

Run the complete LIOS Health Check. This is the canonical full review
and should be the default when Lc wants a comprehensive read on the
portfolio.

## When to use

- Monthly review (always)
- After major market moves (>5% NLV swing)
- Before any Rebalance
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
1. Portfolio Health          — NLV, sleeve totals vs targets, reconciliation
2. Gap & Drift               — one table; gap and drift are the same number
3. Regime Index           — five scores, weighted RI, band, band change
4. Constraint Check          — single name, factor, duration, risky, US-situs
5. Sleeve A Review
6. Sleeve B Review           — LOS, thesis-exit status, slot drift
7. Sleeve C Review           — rotation ranking, ceiling status
8. Sleeve D Review           — duration, ammunition available
9. Sleeve Z Review           — floor and ceiling
10. Top-up Plan              — allocation in dollars
11. Rebalance Status         — triggered / executed / escalated
12. Frontier Scoreboard
13. Execution Calendar
14. Execution Plan
15. Risk Assessment          — the drawdown ladder in dollars
16. CIO Resolution

Rules:
- Conclusions first, reasoning second
- Exact dollar amounts always (never percentages alone)
- Explicit directives: Buy / Hold / Reduce / Exit
- Facts separated from Assessments
- Confidence % on every recommendation in sections 5, 6, 8, 14, 15
- No skipped sections — if no news, state "no action" with one-line rationale
- The RI is a gauge: it may NOT change total risky exposure (P2)
- The only Sleeve B sell trigger is a thesis break, LOS < 60 (P5)
- Money moves only via the three deployment rules (P3)
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
- Sleeve B caps (L3 form; values in `state.yaml: sleeves.b.caps`)
- Kelly v2.0 framework (L4)
- Replacement Index for D (L5)
- SR funding/deployment rules (L6)
- `state.yaml: sleeve_a.permanently_exited` is binding for your portfolio (was L7; portfolio-local since v3.0)

If a directive would violate a Locked Decision, the report must
flag it explicitly and recommend either (a) the framework-compliant
alternative or (b) a constitutional amendment — never silently violate.