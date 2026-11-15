> **v5 conformance note.** Sleeves are **A/B/C/Z** measured against NLV. Sleeve D
> is deleted; duration lives in C. Cash is one sleeve (Z) with no sub-buckets.

# `/portfolio-print` — Canonical Portfolio Export

## Purpose

Export the current portfolio into a clean, standardized Markdown
snapshot suitable for direct copy/paste into any AI session
(ChatGPT, Claude, Gemini, DeepSeek, etc.).

**portfolio-print is an export command only.** It never changes the
portfolio, never executes trades, never updates LIOS rules.

## When to use

- Before starting any new AI session (to port portfolio state)
- After any trade execution (to update the interchange format)
- Before pasting into any LIOS command that expects a snapshot
- Whenever you need to share the portfolio with another AI

## Command

---

```
portfolio print

Perform the following steps in order:

### Step 1 — Retrieve portfolio data

Obtain from your connected brokerage (IBKR or equivalent):
- Net Liquidation Value
- Cash Balance
- Every position:
    - Ticker
    - Current Price
    - Shares
    - Average Cost (if available)
    - Market Value
    - Unrealized P/L (if available)

### Step 2 — Confirm cash

LIOS v5 has one cash sleeve. Do not ask for sub-buckets.

    Cash (Sleeve Z): ____
    Z vehicle(s):    ____   (duration near zero only)

Never assume the value. Always ask.

### Step 3 — Validate cash

`Z` is the entire uninvested balance plus any duration-near-zero instrument. There
is no Strategic Reserve, no Kelly Reserve, no free-tactical bucket, and no ABC
denominator in v5. If a sub-bucket split is offered, reject it and restate that
cash is one sleeve.

Z carries a floor (default 3% of NLV) and a ceiling (`state.yaml: sleeves.z.*`).
Outside that range it is a band breach — the fix is a Rebalance, not a relabelling.

### Step 4 — Classify every holding into its LIOS sleeve

Sleeve A — ETFs:
  (positions filtered to sleeve: a — broad-index AND sector/thematic ETFs. A
   thematic holding carries a written view statement, horizon and invalidation
   condition, held in state; do not invent one)

Sleeve B — Satellites:
  (positions filtered to sleeve: b — individual companies and narrow bets.
   Cap-only: this sleeve has a ceiling, not a target)

Sleeve C — Duration:
  (positions filtered to sleeve: c — bonds, duration greater than 3 years, hedged.
   Duration-near-zero funds do not belong here; they are Z)

Sleeve Z — Cash Equivalents:
  (cash balances plus positions filtered to sleeve: z — duration near zero)

Use the user's existing LIOS sleeve mapping. Do not reclassify
without explicit instruction.

### Step 5 — Generate the Markdown report

Output in this exact format with no extra explanation after the report.

Title: # LIOS Portfolio Snapshot

Metadata block:
  Date: [current date]
  Source: portfolio print
  Sleeves: A / B / C / Z (LIOS v5)
  Net Liquidation Value: [NLV]

Sleeve tables:

## Sleeve A — ETFs
| Ticker | Price | Shares | Avg Cost | Market Value | Unrealized P/L | Weight |
|---|---|---|---|---|---|---|
| ... | ... | ... | ... | ... | ... | ... |
[Sub-total row]

## Sleeve B — Satellites
[Same columns]

## Sleeve C — Duration
[Same columns; add a Duration column in years if available in state]

## Sleeve Z — Cash Equivalents
[Same columns; money-market and T-bill positions render as rows, cash renders as a
 single Cash row]

## Portfolio Summary
| Metric | Value | % of NLV | Target % | Delta pp |
|---|---|---|---|---|
| Sleeve A | ... | ... | ... | ... |
| Sleeve B | ... | ... | no target — ceiling | ... |
| Sleeve C | ... | ... | ... | ... |
| Sleeve Z | ... | ... | ... | ... |
| Risky block (A + B) | ... | ... | per glidepath | ... |
| Defensive block (C + Z) | ... | ... | ... | ... |
| Net Liquidation Value | ... | 100% | — | — |

If avg cost and P/L are unavailable, print those columns as "—"
but keep the column headers (so downstream parsers see consistent
positions).

Output requirements:
- Valid Markdown
- Human readable
- AI readable
- Ready for direct copy/paste
- No extra explanation after the report
- Consistent formatting every execution

Begin.
```

---

## What this command produces

A standardized Markdown portfolio snapshot that:
- Splits holdings by the four v5 sleeves, A / B / C / Z
- Includes Average Cost + Unrealized P/L (when available)
- Sorts holdings by sleeve
- Shows each sleeve against its target, and Sleeve B against its ceiling
- Is the canonical AI-readable interchange format for all LIOS commands

## Design principle

This Markdown format is the **canonical portfolio interchange
format** for LIOS. Every command that consumes a snapshot — health-check,
drift-check, sleeve reviews, replacement analysis — accepts the
portfolio-print output directly, without manual reformatting.

## Related

- See `../core/sleeves.md` — sleeve definitions
- See `../constitution/constitution.md` — what may not change
- See `snapshot.md` — simpler raw-to-snapshot converter (no avg cost / P/L prompt)
- See `portfolio-los.md` — LOS-only dashboards (complementary, not overlapping)

## Discipline reminders

- This is an **export-only** command. Never execute trades.
- Cash allocation is **user-provided**, never assumed.
- Cash is one sleeve. No Strategic Reserve, no Kelly Reserve, no free-tactical
  bucket — those are deleted concepts, not categories.
- Sleeve B has a ceiling, not a target. Print it that way.
- If avg cost / P/L are unavailable from brokerage, omit them with
  "—" but keep column positions for parser consistency.
- The output must be immediately pasteable into any LLM as-is.
