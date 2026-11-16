> **v6 conformance note.** Sleeves are **A/B/C** measured against NLV. Sleeve D
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

LIOS v6 has one defensive sleeve. Do not ask for sub-buckets, and do not ask for a
cash target — there isn't one.

    Cash balance: ____   (part of sleeve C)

Never assume the value. Always ask.

### Step 3 — Validate cash

Cash is part of the defensive sleeve. There is no Strategic Reserve, no Kelly
Reserve, no free-tactical bucket, no ABC denominator, and since v6 no separate cash
sleeve. If a cash sub-bucket or a cash target is offered, reject it: composition is
reported, never targeted.

C carries one control — average duration 1–6 years, no single holding above 10. That
is a measurement, not a target, so the fix for a breach is a rotation inside C.

### Step 4 — Classify every holding into its LIOS sleeve

Sleeve A — ETFs:
  (positions filtered to sleeve: a — broad-index AND sector/thematic ETFs. A
   thematic holding carries a written view statement, horizon and invalidation
   condition, held in state; do not invent one)

Sleeve B — Satellites:
  (positions filtered to sleeve: b — individual companies and narrow bets.
   Cap-only: this sleeve has a ceiling, not a target)

Sleeve C — Defensive:
  (positions filtered to sleeve: c — cash, money market, T-bills, bond ETFs of any
   duration. Ask for one Duration (years) per holding, from the fact sheet. There is
   no cash/bond split to confirm: composition is reported, never targeted.)

Use the user's existing LIOS sleeve mapping. Do not reclassify
without explicit instruction.

### Step 5 — Generate the Markdown report

Output in this exact format with no extra explanation after the report.

Title: # LIOS Portfolio Snapshot

Metadata block:
  Date: [current date]
  Source: portfolio print
  Sleeves: A / B / C (LIOS v6)
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
[Same columns, plus Duration (years) per holding. Add a summary: weighted average
 duration against the 1–6y band, money duration in dollars, and the cost of a 1pp
 move. Cash renders as one row at duration 0. Say so explicitly if the composition
 is a barbell — bills plus long bonds — even when the average is inside the band]

## Portfolio Summary
| Metric | Value | % of NLV | Target % | Delta pp |
|---|---|---|---|---|
| Sleeve A | ... | ... | ... | ... |
| Sleeve B | ... | ... | no target — ceiling | ... |
| Sleeve C | ... | ... | residual | ... |
| Risky block (A + B) | ... | ... | per glidepath | ... |
| Defensive block (C) | ... | ... | residual | ... |
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
- Splits holdings by the three v6 sleeves, A / B / C
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
