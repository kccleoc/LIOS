> **v7 conformance note.** Sleeves are **A/B/C** measured against NLV.
> Reports are **14 sections**, in the fixed P8′ order. The prompt
> below is self-contained — paste it as-is.

# `/live-health-check` — Live-Market 14-Section Report

## Purpose

Run the complete LIOS Health Check using **live market data** — real-time prices plus current news and macro context. No stale snapshot pricing.

The same canonical 14-section report as `/health-check`, but every dollar figure is marked to market at the moment the command runs.

## When to use

- Any time you want the portfolio assessed at **right-now** prices and context
- After the standard `/health-check` if you want a live-price overlay
- Before making any buy/sell decision where stale snapshot prices could mislead
- On days with significant market movement before Investment Committee decisions

## Inputs required

1. **Portfolio snapshot** (paste from `snapshots/` — provides shares, sleeves, caps, targets)
2. Any context the owner wants surfaced (specific concerns, open questions)

The command does NOT require you to provide current prices or market context. It fetches everything it needs from live sources.

## Command

Paste the following into a LIOS-initialized session, after providing the portfolio snapshot:

---

```
Run a full LIOS Health Check on my portfolio using LIVE market data (LIOS v7).

Snapshot:
<paste snapshot here>

First confirm framework/VERSION is 7.0.0, then run all 30 validation
checks in order (data checks 1–12 abort on failure; constitutional
checks 13–30 report, never abort). Never generate a report from an
unvalidated state.

Sleeve definitions. There are three:
  A — ETFs, broad-index AND sector/thematic (every holding needs a written view statement)
  B — satellites: individual companies and narrow bets. CAP-ONLY, ceiling 10% of NLV
  C — defensive: cash, money market, T-bills, and bond ETFs of any duration.
      RESIDUAL (C = NLV − A − B). One control: average duration 1–6 years,
      no single holding above 10y, holdings above 6y at or under 50% of C.

A + B is the risky block. C is the defensive block. One denominator: NLV.

=== PHASE 1 — FETCH LIVE PRICES ===

Step 1. Fetch current prices for every symbol below.
For each symbol S: fetch the quote page for S and extract the current price.

Symbols to fetch (derived from `portfolio/state/portfolio.yaml: positions`; substitute your actual tickers below):

  <TICKER-A1>  — Sleeve A       (e.g., broad-index equity ETF)
  <TICKER-A2>  — Sleeve A       (e.g., sector or thematic ETF)
  <TICKER-A3>  — Sleeve A       (etc.; one line per holding with sleeve: A)
  <TICKER-B1>  — Sleeve B       (satellite; one line per holding with sleeve: B)
  <TICKER-B2>  — Sleeve B
  <TICKER-B3>  — Sleeve B
  <TICKER-C1>  — Sleeve C       (bond ETF; report duration_years and its fact-sheet source)
  <TICKER-C2>  — Sleeve C       (etc.; one line per holding with sleeve: C)

For non-US listings append the local exchange suffix. Cash balances come from the
snapshot, not from a fetch.

Step 2. If any fetch fails: note the symbol, use the snapshot price for that holding,
and flag it in the Delta Summary. Do NOT stop — fail-soft.

Step 3. Recalculate for each holding:
  Market Value = Shares (from snapshot) * Live Price
  Sleeve MV = sum of holding MVs in that sleeve
  NLV = Total Gross MV + Cash (from snapshot)
  Sleeve % of NLV = Sleeve MV / NLV * 100
  Gap $ = Target $ − Sleeve MV          for A and C only (T_A = risky_target × NLV' − B_actual; T_C = (1 − risky_target) × NLV')
  Gap % = Gap $ / NLV * 100
  Ceiling headroom $ = (NLV * B ceiling %) - B MV  for B; there is no B target
  Position cap proximity = Holding MV / (NLV * position cap %) * 100

=== PHASE 2 — FETCH LIVE CONTEXT ===

For each Sleeve B holding listed in `portfolio/state/portfolio.yaml: positions`:
  Search web for "TICKER stock news <current month>" — summarize in 1-2 sentences:
  earnings, guidance, major announcements, analyst moves.

For each Sleeve C holding: any issuer, rating, or duration-relevant news. No
earnings sweep — C is not scored for quality.

One macro search: "<index> market news <current month>" — summarize key events
(central bank, inflation, earnings season, rate moves).

=== PHASE 3 — DELTA SUMMARY (OUTPUT FIRST) ===

Before the 14 sections, output a **Changes Since Snapshot** block:

  Snapshot date: <date from snapshot>
  Snapshot NLV: <snapshot NLV>
  Live NLV: <recalculated NLV>
  NLV change: <delta> (<pct>%)
  Biggest mover: <TICKER> (<snapshot price> -> <live price>, <pct>%)
  Cash: <snapshot cash> (unchanged)
  Failed fetches: <list any> — using snapshot prices

=== PHASE 4 — 14-SECTION HEALTH CHECK ===

Output the complete 14-section report using live prices and fresh context.
Fixed order, no deviations:

 1. Portfolio Health             — summary table + 3-line headline (live NLV, what changed, what matters)
 2. Gap & Drift                  — one table; gap and drift are the same number; B shown against its ceiling
 3. Regime Index                 — five dimension scores + band + band change (dashboard; no mechanical effect; not evidence)
 4. Constraint Check             — look-through name, sector, duration, risky, true US-situs, B ceiling, B count, thematic block
 5. Sleeve A Review              — live MV, within-A core vs thematic, view statement and invalidation
                                   status per holding, `portfolio/state/portfolio.yaml: sleeves.a.permanently_exited`
 6. Sleeve B Review              — live MV, % NLV, ceiling status in dollars, written thesis and
                                   thesis-break status, name count
 7. Sleeve C Review              — live MV, duration, ammunition available for Rebalance
 8. Top-up Plan                  — allocation in dollars; state risky_now = (A + B) / NLV and whether
                                   the risky-cap branch applies; state that B receives $0.00
 9. Rebalance Status             — triggered? hard or soft? executed? escalated under P7′?
10. Look-Through Concentration   — top underlying companies across every wrapper, in dollars and % NLV
11. Satellite Review             — B names: thesis, evidence, review status — not a score, not a ranking
12. Execution Calendar           — B earnings, A index and distribution events, macro, mandate windows, next quarterly review
13. Execution Plan               — BUY/HOLD/REDUCE/EXIT with dollars, confidence %, rule cited, each sale tagged to a P4′ direction and an operation
14. Risk Assessment & CIO Resolution — drawdown ladder in dollars, then one paragraph

=== RULES ===
- Conclusions first, reasoning second
- Exact dollar amounts always — never percentages alone
- Explicit directives: Buy / Hold / Reduce / Exit / Pass / Watch
- Facts separated from Assessments in every section
- Confidence % on every recommendation
- No skipped sections — if no news, state "no action" with one-line rationale
- risky_now = (A + B) / NLV. C is defensive
- Sleeve B has a ceiling, not a target; the distance to that ceiling is not fundable
- There is no score in V7. The only Sleeve B exit is a thesis break, established by
  documented research and verification, decided at the quarterly review. A trim is
  permitted only at a Quarterly Review when B exceeds its ceiling beyond tolerance,
  and never below 1.0% of NLV
- C is bought, not rotated for performance. No ranking inside C
- Money moves by two operations only: Top-up and Rebalance (plus a time-boxed
  transition mandate, windowed, when the structure itself changes)
- A to B is an Investment Committee decision; A to C is prohibited; exit proceeds go to A, never to C directly
- The US-situs ceiling is tested on the TRUE figure (book + listed external holdings)
- Every directive in section 13 cites the rule that produced it
- End with CIO Resolution as one paragraph
```

---

## What you get back

A complete 14-section report in the fixed P8′ order, preceded by a
Delta Summary. Every dollar figure is marked to market at the moment
the command runs. Structure is specified in `../core/health-check.md`.
Section 13 (Execution Plan) is the actionable core.

## What the prompt enforces

Every directive in section 13 must trace to a rule:

- **P1′** — three sleeves, one denominator (NLV); scope: the book is the IBKR portfolio, Mox securities listed with their US-situs total
- **P2′** — targets from the glidepath; risky = A + B; B is cap-only for life
- **P3′** — two operations only (Top-up, Rebalance); hard caps fire immediately, cure or escalate within 10 business days
- **P4′** — direction limits: C → A ✅ · B → A ✅ · rotation inside C is not a transfer · A → B IC only · A → C ❌ · B → C ❌
- **P5′** — exits follow the instrument; a thesis break needs research and verification; replacement with the same exposure is a Rebalance, not an exit
- **P6′** — look-through company 5%, sector 25% on GICS industry group, B ceiling 10%, B name count 5, thematic block 25% of A, C duration 1–6y average and no single holding above 10y and holdings above 6y at or under 50% of C, US-situs below the $60,000 exemption (true figure)
- **P7′** — hard breach is immediate, soft breach waits for the quarterly review; an accepted breach expires
- `portfolio/state/portfolio.yaml: sleeves.a.permanently_exited` is binding

If a directive would breach a cap, **the cap wins**. Report the
framework-compliant alternative — never silently violate.
