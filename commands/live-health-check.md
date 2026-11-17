> **v6 conformance note.** Sleeves are **A/B/C** measured against NLV. Sleeve D
> is deleted; duration lives in C. Reports are **14 sections**, not 16. The prompt
> below is self-contained — paste it as-is.

# `/live-health-check` — Live-Market 15-Section Report

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
2. Any context Lc wants surfaced (specific concerns, open questions)

The command does NOT require you to provide current prices or market context. It fetches everything it needs from live sources.

## Command

Paste the following into a LIOS-initialized session, after providing the portfolio snapshot:

---

```
Run a full LIOS Health Check on my portfolio using LIVE market data (LIOS v6).

Snapshot:
<paste snapshot here>

Sleeve definitions. There are four, and no Sleeve D:
  A — ETFs, broad-index AND sector/thematic (thematic needs a written view statement)
  B — satellites: individual companies and narrow bets. CAP-ONLY, ceiling 10% of NLV
  C — bonds, duration greater than 3 years, hedged
  C — defensive: cash, money market, T-bills, bonds of any duration

A + B is the risky block. C is the defensive block. One denominator: NLV.

=== PHASE 1 — FETCH LIVE PRICES ===

Step 1. Fetch current prices for every symbol below.
For each symbol S: fetch the quote page for S and extract the current price.


Symbols to fetch (derived from `state.yaml: positions`; substitute your actual tickers below):

  <TICKER-A1>  — Sleeve A       (e.g., broad-index equity ETF)
  <TICKER-A2>  — Sleeve A       (e.g., sector or thematic ETF)
  <TICKER-A3>  — Sleeve A       (etc.; one line per holding with sleeve: A)
  <TICKER-B1>  — Sleeve B       (satellite; one line per holding with sleeve: B)
  <TICKER-B2>  — Sleeve B
  <TICKER-B3>  — Sleeve B
  <TICKER-C1>  — Sleeve C       (duration greater than 3y, hedged)
  <TICKER-C2>  — Sleeve C       (etc.; one line per holding with sleeve: C)
  <TICKER-C2>  — Sleeve C       (e.g. a T-bill fund; report duration_years)

For non-US listings append the local exchange suffix. Cash balances come from the
snapshot, not from a fetch.

Step 2. If any fetch fails: note the symbol, use the snapshot price for that holding,
and flag it in the Delta Summary. Do NOT stop — fail-soft.

Step 3. Recalculate for each holding:
  Market Value = Shares (from snapshot) * Live Price
  Sleeve MV = sum of holding MVs in that sleeve
  NLV = Total Gross MV + Cash (from snapshot)
  Sleeve % of NLV = Sleeve MV / NLV * 100
  Drift $ = Sleeve MV - (NLV * Target %)          for A and C only
  Drift % = Drift $ / NLV * 100
  Ceiling headroom $ = (NLV * B ceiling %) - B MV  for B; there is no B target
  Position cap proximity = Holding MV / (NLV * position cap %) * 100


=== PHASE 2 — FETCH LIVE CONTEXT ===

For each Sleeve B holding listed in `state.yaml: positions`:
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

=== PHASE 4 — 15-SECTION HEALTH CHECK ===

Output the complete 14-section report using live prices and fresh context.
Fixed order, no deviations:

 1. Portfolio Health             — summary table + 3-line headline (live NLV, what changed, what matters)
 2. Gap & Drift                  — one table; gap and drift are the same number; B shown against its ceiling
 3. Regime Index                 — five dimension scores + band + band change (dashboard; no mechanical effect)
 4. Constraint Check             — look-through name, sector, duration, risky, US-situs, B ceiling
 5. Sleeve A Review              — live MV, within-A core vs thematic, view statement and invalidation
                                   status per thematic holding, `state.yaml: sleeve_a.permanently_exited`
 6. Sleeve B Review              — live MV, % NLV, ceiling status in dollars, LOS and components,
                                   thesis-exit status, replacement ranking by RI, directive
 7. Sleeve C Review              — live MV, duration, currency hedge, ammunition available for Rebalance
 8. Top-up Plan                  — allocation in dollars; state risky_now = (A + B) / NLV and whether
                                   the risky-cap branch applies; state that B receives $0.00
9. Rebalance Status             — triggered? hard or soft? executed? escalated under P7'?
10. Look-Through Concentration   — top underlying companies across every wrapper, in dollars and % NLV
11. Satellite Scoreboard         — B names by Replacement Index, watchlist candidates, weakest named
12. Execution Calendar           — B earnings, A index and distribution events, macro, next quarterly review
13. Execution Plan               — BUY/HOLD/REDUCE/EXIT with dollars, confidence %, rule cited
14. Risk Assessment & CIO Resolution — drawdown ladder in dollars, then one paragraph


=== RULES ===
- Conclusions first, reasoning second
- Exact dollar amounts always — never percentages alone
- Explicit directives: Buy / Hold / Reduce / Exit / Pass / Watch
- Facts separated from Assessments in every section
- Confidence % on every recommendation
- No skipped sections — if no news, state "no action" with one-line rationale
- risky_now = (A + B) / NLV. C is bonds
- Sleeve B has a ceiling, not a target; the distance to that ceiling is not fundable
- The only Sleeve B exit is a thesis break. LOS below 60 opens a 30-day review
  resolving to a re-score of 60 or above, or exit
- C is bought, not rotated. No ranking, no Replacement Index inside C
- Money moves by two operations only: Top-up and Rebalance
- A to B is an Investment Committee decision; A to C is prohibited
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
