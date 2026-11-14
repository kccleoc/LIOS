> **v3.0 conformance note.** Written for LIOS v2.x. In v3.0 the sleeves are
> **A/B/C/D/Z** measured against NLV, the Kelly Factor is replaced by the
> **Bull–Bear Index**, and the Strategic Reserve is repealed. Where this file
> and `lios/v3.0.md` disagree, v3.0 wins.


# `/live-health-check` — Live-Market 16-Section Report

## Purpose

Run the complete LIOS Health Check using **live market data** — real-time prices from Yahoo Finance plus current news and macro context. No stale snapshot pricing.

The same canonical 16-section report as `/health-check`, but every dollar figure is marked to market at the moment the command runs.


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
Run a full LIOS Health Check on my portfolio using LIVE market data.

Snapshot:
<paste snapshot here>

=== PHASE 1 — FETCH LIVE PRICES ===

Step 1. Fetch current prices from Yahoo Finance for every symbol below.
For each symbol S: fetch finance.yahoo.com/quote/S and extract the current price.


Symbols to fetch (derived from `state.yaml: positions`; substitute your actual tickers below):

  <TICKER-A1>  — Sleeve A       (e.g., all-world equity ETF)
  <TICKER-A2>  — Sleeve A       (e.g., S&P 500 concentration ETF)
  <TICKER-A3>  — Sleeve A       (e.g., short-duration Treasury ETF)
  <TICKER-B1>  — Sleeve B       (first high-conviction holding)
  <TICKER-B2>  — Sleeve B       (second holding; etc.)
  <TICKER-B3>  — Sleeve B
  <TICKER-D1>  — Sleeve D       (frontier holding, stage 1/2/3)
  <TICKER-D2>  — Sleeve D
  <TICKER-D3>  — Sleeve D       (etc.; one line per `state.yaml: positions` row with sleeve: D)
  ^GSPC         — S&P 500 index (for Trigger Check; remove if your reference indices differ)
  ^IXIC         — Nasdaq index (for Trigger Check; remove if your reference indices differ)

For US tickers use the bare symbol; for LSE-listed tickers append `.L`.

Step 2. If any fetch fails: note the symbol, use the snapshot price for that holding,
and flag it in the Delta Summary. Do NOT stop — fail-soft.

Step 3. Recalculate for each holding:
  Market Value = Shares (from snapshot) * Live Price
  Sleeve MV = sum of holding MVs in that sleeve
  NLV = Total Gross MV + Cash (from snapshot)
  Sleeve % of NLV = Sleeve MV / NLV * 100
  Drift $ = Sleeve MV - (NLV * Target %)
  Drift % = Drift $ / NLV * 100
  Cap proximity = Holding MV / (NLV * Cap) * 100


=== PHASE 2 — FETCH LIVE CONTEXT ===

For each Sleeve B and D holding listed in `state.yaml: positions`:
  Search web for "TICKER stock news <current month>" — summarize in 1-2 sentences:
  earnings, guidance, major announcements, analyst moves.

One macro search: "S&P 500 Nasdaq market news July 2026" — summarize key events
(Fed, CPI, earnings season, sector rotations).

=== PHASE 3 — DELTA SUMMARY (OUTPUT FIRST) ===

Before the 16 sections, output a **Changes Since Snapshot** block:

  Snapshot date: <date from snapshot>
  Snapshot NLV: $<snapshot NLV>
  Live NLV: $<recalculated NLV>
  NLV change: $<delta> (<pct>%)
  Biggest mover: <TICKER> (<snapshot price> -> <live price>, <pct>%)
  Cash: $<snapshot cash> (unchanged)
  Failed fetches: <list any> — using snapshot prices

=== PHASE 4 — 16-SECTION HEALTH CHECK ===

Output the complete 16-section report using live prices and fresh context.
Follow the canonical order and format from
`../lios/components/06_health_check_structure.md`.

Sections:
1. Portfolio Health — summary table + 3-line headline (live NLV, what changed, what matters)
2. Drift Check — table with live drift $ and %, flag >2%
3. Kelly Dashboard — five-dimension scores + Kelly Factor (incorporate fresh macro)
4. Trigger Check — S&P/Nasdaq correction scan (use live ^GSPC, ^IXIC), other triggers
5. Sleeve A Review — live MV, drift, `state.yaml: sleeve_a.permanently_exited` flag (was L7), no LOS/LFS (index ETFs)
6. Sleeve B Review — live MV, %NLV, cap proximity, LOS, LFS, Decision Matrix, directive
7. Sleeve C Review — Kelly Reserve, tactical deployments, drift
8. Sleeve D Review — stage, Replacement Index, LOS, LFS, Decision Matrix, verdict
9. Strategic Reserve Review — balance, last deployment, opportunity set
10. SR Opportunity Ranking — table with Expected Return, Kelly Edge, Valuation, Diversification, LOS, LFS, Action
11. Frontier Scoreboard — all D + watchlist, RI composite, LOS, LFS
12. Technology Wave Map — active waves, exposure via B+D, gaps
13. Execution Calendar — upcoming earnings (B/D), macro events (next 30 days)
14. Execution Plan — BUY/HOLD/REDUCE/EXIT with $, Confidence %, rationale
15. Risk Assessment — concentration, correlation, economic engine gaps, scenario stress
16. CIO Resolution — one paragraph, conclusions first, dollars, confidence


=== RULES ===
- Conclusions first, reasoning second
- Exact dollar amounts always — never percentages alone
- Explicit directives: Buy / Hold / Reduce / Exit
- Facts separated from Assessments in every section
- Confidence % on every recommendation in sections 5, 6, 8, 14, 15
- No skipped sections — if no news, state "no action" with one-line rationale
- End with CIO Resolution as one paragraph
- Any directive violating a Locked Decision must be flagged with compliant alternative
```

---

## What you get back

A complete 16-section report following the spec in
`../lios/components/06_health_check_structure.md`, preceded by a
Delta Summary. Every dollar figure is marked to market at the moment
the command runs. Section 14 (Execution Plan) is the actionable core.

## Respect locked decisions

Every directive in section 14 must respect:
- Sleeve A targets (L2 form; values in `state.yaml: sleeve_a.allocation`)
- Sleeve B per-position caps (L3 form; values in `state.yaml: sleeve_b_caps`)
- Kelly v2.0 framework (L4)
- Replacement Index for D (L5)
- SR funding/deployment rules (L6)
- `state.yaml: sleeve_a.permanently_exited` list is binding for your portfolio (was L7; portfolio-local discipline since v3.0)

If a directive would violate a Locked Decision, the report must
flag it explicitly and recommend either (a) the framework-compliant
alternative or (b) a constitutional amendment — never silently violate.
