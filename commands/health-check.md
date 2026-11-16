> **v5 conformance note.** Sleeves are **A/B/C** measured against NLV. Sleeve D
> is deleted; duration lives in C. Reports are **14 sections**, not 16. The prompt
> below is self-contained — paste it as-is.

# `/health-check` — Full 14-Section Report

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
Run a full LIOS Health Check on my portfolio (LIOS v5).

Snapshot:
<paste live snapshot here>

Output the complete 14-section report in the fixed order:

 1. Portfolio Health             — NLV, sleeve totals vs targets, reconciliation
 2. Gap & Drift                  — one table; gap and drift are the same number
 3. Regime Index                 — five scores, weighted RI, band, band change (dashboard)
 4. Constraint Check             — look-through name, sector, duration, risky, US-situs, B ceiling
 5. Sleeve A Review              — holdings, view statements, within-A split, thematic drift
 6. Sleeve B Review              — LOS, thesis-exit status, ceiling status, replacement ranking
 7. Sleeve C Review              — composition, duration table, money duration, ammunition
 8. Top-up Plan                  — allocation in dollars, A / C / Z only
9. Rebalance Status             — triggered? hard or soft? executed? escalated?
10. Look-Through Concentration   — top names and sectors across every wrapper
11. Satellite Scoreboard         — B names by Replacement Index, plus candidates
12. Execution Calendar           — earnings, index events, macro, next quarterly review
13. Execution Plan               — Buy / Hold / Reduce / Exit with dollars
14. Risk Assessment & CIO Resolution — drawdown ladder in dollars, then one paragraph

Sleeve definitions. There are three, and no Sleeve D:
  A — ETFs, broad-index AND sector/thematic. Every thematic holding needs a
      written view statement, a horizon, and an invalidation condition.
  B — satellites: individual companies and narrow bets. CAP-ONLY, ceiling 10% of NLV.
      No target weight. No equal weight at entry.
  C — defensive: cash, money market, T-bills, and bond ETFs of any duration.
      RESIDUAL target (C = NLV − A − B). One control: average duration 1–6 years,
      no single holding above 10. No internal cash/bond split — composition is
      reported, never targeted.

A + B is the risky block. C is the defensive block.
One denominator: NLV. No second denominator.

Rules:
- Conclusions first, reasoning second
- Exact dollar amounts always — never percentages alone
- Explicit directives: Buy / Hold / Reduce / Exit / Pass / Watch
- Facts separated from Assessments
- Confidence % on every recommendation
- No skipped sections — if no news, state "no action" with one-line rationale
- risky_now = (A + B) / NLV. C is bonds. Counting C as risky is an error and
  must be flagged if you catch it in any input
- Sleeve B has a ceiling, not a target. Report B's distance to the ceiling in
  dollars; that distance is not fundable
- Sleeve B receives $0.00 from this month's Top-up. State that line explicitly
- The Regime Index is a dashboard. It sizes nothing, shifts no target, and
  gates no operation. State this each cycle
- The only Sleeve B exit is a thesis break. LOS below 60 opens a 30-day
  deep-dive review that resolves to a documented re-score of 60 or above, or
  exit. No third outcome. Price and drawdown are never exit triggers
- An ETF is not sold because a constituent company's thesis breaks. That
  triggers a look-through concentration review, not a sale
- Money moves by two operations only: Top-up and Rebalance
- A to B is an Investment Committee decision, never an automatic Rebalance
  output, and is limited by B's ceiling headroom. A to C is prohibited
- Deviations inside rebalance_tolerance_pct are reported, not traded
- Every directive in section 14 must cite the rule that produced it. An action
  with no rule citation is deleted from the plan
- End with CIO Resolution as one paragraph

Begin.
```

---

## What you get back

A complete 14-section report in the fixed P8′ order, specified in
`../core/health-check.md` and rendered from `../templates/HealthCheck.md`.
Section 14 (Execution Plan) is the actionable core — that's where the
Buy / Hold / Reduce / Exit directives live.

## What the prompt enforces

Every directive in section 14 must trace to a rule:

- **P1′** — three sleeves, one denominator (NLV)
- **P2′** — targets from the glidepath; risky = A + B; B is cap-only
- **P4′** — direction limits: C → A ✅ · B → A ✅ · rotation inside C is not a transfer · A → B IC only · A → C ❌
- **P5′** — exits follow the instrument; replacement is not an exit
- **P6′** — look-through company 5%, sector 25%, B ceiling 10%, C 15%,
  C duration 1–6y average and no single holding above 10y, US-situs below exemption
- **P7′** — hard breach is immediate, soft breach waits for the quarterly review
- `state.yaml: sleeve_a.permanently_exited` is binding for your portfolio

If a directive would breach a cap, **the cap wins**. Report the
framework-compliant alternative — never silently violate.
