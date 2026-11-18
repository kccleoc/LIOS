> **v7 conformance note.** Sleeves are **A/B/C** measured against NLV.
> Reports are **14 sections**, in the fixed P8′ order. The prompt
> below is self-contained — paste it as-is.

# `/health-check` — Full 14-Section Report

## Purpose

Run the complete LIOS Health Check. This is the canonical full review
and should be the default when the owner wants a comprehensive read on
the portfolio.

## When to use

- Monthly review (always)
- After major market moves (>5% NLV swing)
- Before any Rebalance
- Quarterly as a full reset

## Inputs required

1. **Live portfolio snapshot** (paste from `snapshots/`)
2. Current market data (or note that AI may use most recent available)
3. Any context the owner wants surfaced (specific concerns, open questions)

## Command

Paste the following into a LIOS-initialized session, after providing
the live snapshot:

---

```
Run a full LIOS Health Check on my portfolio (LIOS v7).

Snapshot:
<paste live snapshot here>

First confirm framework/VERSION is 7.0.0, then run all 30 validation
checks in order (data checks 1–12 abort on failure; constitutional
checks 13–30 report, never abort). Never generate a report from an
unvalidated state.

Output the complete 14-section report in the fixed order:

 1. Portfolio Health             — NLV, sleeve totals vs targets, reconciliation
 2. Gap & Drift                  — one table; gap and drift are the same number
 3. Regime Index                 — five scores, weighted RI, band, band change (dashboard)
 4. Constraint Check             — look-through name, sector, duration, risky, true US-situs, B ceiling, B count, thematic block
 5. Sleeve A Review              — holdings, view statements, within-A split, thematic drift; the A view-statement re-read
 6. Sleeve B Review              — written theses, thesis-break status, ceiling status, name count
 7. Sleeve C Review              — composition, duration table, money duration, ammunition
 8. Top-up Plan                  — allocation in dollars, A / C only, never B
 9. Rebalance Status             — triggered? hard or soft? executed? escalated?
10. Look-Through Concentration   — top names and sectors across every wrapper
11. Satellite Review             — B names: thesis, evidence, review status — not a score, not a ranking
12. Execution Calendar           — earnings, index events, macro, mandate windows, next quarterly review
13. Execution Plan               — Buy / Hold / Reduce / Exit with dollars, each tagged to a P4′ direction and an operation
14. Risk Assessment & CIO Resolution — drawdown ladder in dollars, then one paragraph

Sleeve definitions. There are three:
  A — ETFs, broad-index AND sector/thematic. Every holding needs a
      written view statement, a horizon, and an invalidation condition.
  B — satellites: individual companies and narrow bets. CAP-ONLY, ceiling 10% of NLV.
      No target weight. No equal weight at entry.
  C — defensive: cash, money market, T-bills, and bond ETFs of any duration.
      RESIDUAL (C = NLV − A − B). One control: average duration 1–6 years,
      no single holding above 10y, holdings above 6y at or under 50% of C.
      No internal cash/bond split — composition is reported, never targeted.

A + B is the risky block. C is the defensive block.
One denominator: NLV. No second denominator.

Rules:
- Conclusions first, reasoning second
- Exact dollar amounts always — never percentages alone
- Explicit directives: Buy / Hold / Reduce / Exit / Pass / Watch
- Facts separated from Assessments
- Confidence % on every recommendation
- No skipped sections — if no news, state "no action" with one-line rationale
- risky_now = (A + B) / NLV. C is defensive. Counting C as risky is an error and
  must be flagged if you catch it in any input
- Sleeve B has a ceiling, not a target. Report B's distance to the ceiling in
  dollars; that distance is not fundable
- Sleeve B receives $0.00 from this month's Top-up. State that line explicitly
- The Regime Index is a dashboard. It sizes nothing, shifts no target, and
  gates no operation. It is not evidence for a thesis break or a replacement
  decision. State this each cycle
- There is no score in V7. The only Sleeve B exit is a thesis break,
  established by documented research and verification, decided at the
  quarterly review. Price and drawdown are never exit triggers. A trim is
  permitted only at a Quarterly Review when B exceeds its ceiling beyond
  tolerance, and never below 1.0% of NLV
- An ETF is not sold because a constituent company's thesis breaks. That
  triggers a look-through concentration review, not a sale
- Money moves by two operations only: Top-up and Rebalance (plus a
  time-boxed transition mandate, windowed, when the structure itself changes)
- A to B is an Investment Committee decision, never an automatic Rebalance
  output, and is limited by B's ceiling headroom. A to C is prohibited.
  Exit proceeds go to A, never to C directly
- Deviations inside rebalance_tolerance_pct are reported, not traded
- The US-situs ceiling is tested on the TRUE figure (book + listed external
  holdings). At or above the $48,000 stop-adding line, A's share buys only
  A's non-US-domiciled lines
- Every directive in section 13 must cite the rule that produced it. An action
  with no rule citation is deleted from the plan. Every sale is tagged with
  its P4′ direction and its operation; an untagged sale is a breach
- End with CIO Resolution as one paragraph

Begin.
```

---

## What you get back

A complete 14-section report in the fixed P8′ order, specified in
`../core/health-check.md` and rendered from `../templates/HealthCheck.md`.
Section 13 (Execution Plan) is the actionable core — that's where the
Buy / Hold / Reduce / Exit directives live.

## What the prompt enforces

Every directive in section 13 must trace to a rule:

- **P1′** — three sleeves, one denominator (NLV); scope: the book is the
  IBKR portfolio, Mox securities listed with their US-situs total
- **P2′** — targets from the glidepath; risky = A + B; B is cap-only for life
- **P3′** — two operations only (Top-up, Rebalance), soft limits quarterly
  within tolerance; hard caps fire immediately, cure or escalate within
  10 business days
- **P3a** — transition mandate: windowed, 92-day maximum sunset, at most
  three raised in any actual calendar year, 90-day minimum interval
- **P4′** — direction limits: C → A ✅ · B → A ✅ · rotation inside C is not a transfer · A → B IC only · A → C ❌ · B → C ❌
- **P5′** — exits follow the instrument; a thesis break needs research and
  verification; replacement with the same exposure is a Rebalance, not an exit
- **P6′** — look-through company 5%, sector 25% on GICS industry group,
  B ceiling 10%, B name count 5, thematic block 25% of A, C duration 1–6y
  average and no single holding above 10y and holdings above 6y at or under
  50% of C, US-situs below the $60,000 exemption (true figure)
- **P7′** — hard breach is immediate, soft breach waits for the quarterly review;
  an accepted breach expires; a breach accepted twice without a cure leaves
  only cure or constitutional amendment
- `portfolio/state/portfolio.yaml: sleeves.a.permanently_exited` is binding

If a directive would breach a cap, **the cap wins**. Report the
framework-compliant alternative — never silently violate.
