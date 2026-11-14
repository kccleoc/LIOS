> **v3.0 conformance note.** Written for LIOS v2.x. In v3.0 the sleeves are
> **A/B/C/D/Z** measured against NLV, the Kelly Factor is replaced by the
> **Bull–Bear Index**, and the Strategic Reserve is repealed. Where this file
> and `lios/v3.0.md` disagree, v3.0 wins.


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

## Patch version

LIOS Upgrade Patch v1.0 — additive. Does not modify existing rules,
decision logic, Kelly framework, sleeve allocation, or investment
philosophy. Respects L1–L14.

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

### Step 2 — Ask for cash allocation

Prompt the user:

LIOS Cash Allocation
Please provide:
  Strategic Reserve (SR): $
  Kelly Reserve (KR):    $
  Current Month
  Undeployed DCA:        $

Never assume these values. Always ask.

### Step 3 — Validate cash allocation

Free Tactical Cash = Broker Cash - SR - KR - Current Month Undeployed DCA

If allocation exceeds available cash: STOP and report the discrepancy.
Otherwise calculate Free Tactical Cash automatically.

### Step 4 — Classify every holding into its LIOS sleeve

Sleeve A — Core Compounding:
  (tickers from `state.yaml: positions` filtered to sleeve: A)

Sleeve B — Proven Alpha:
  (tickers from `state.yaml: positions` filtered to sleeve: B)

Sleeve D — Frontier Innovation:
  (tickers from `state.yaml: positions` filtered to sleeve: D; include `stage` field)

Sleeve C — Tactical Cash:
  Kelly Reserve + Current Month Undeployed DCA + Free Tactical Cash

Use the user's existing LIOS sleeve mapping. Do not reclassify
without explicit instruction.

### Step 5 — Generate the Markdown report

Output in this exact format with no extra explanation after the report.

Title: # LIOS Portfolio Snapshot

Metadata block:
  Date: [current date]
  Source: portfolio print
  Net Liquidation Value: $X

Sleeve tables:

## Sleeve A — Core Compounding
| Ticker | Price | Shares | Avg Cost | Market Value | Unrealized P/L | Weight |
|---|---|---|---|---|---|---|---|
| ... | ... | ... | ... | ... | ... | ... |
[Sub-total row]

## Sleeve B — Proven Alpha
[Same columns]

## Sleeve D — Frontier Innovation
[Same columns]

## Cash Allocation
| Bucket | Amount |
|---|---|
| Strategic Reserve | $ |
| Kelly Reserve | $ |
| Current Month Undeployed DCA | $ |
| Free Tactical Cash | $ |

## Portfolio Summary
| Metric | Value |
|---|---|
| Sleeve A | $X (XX%) |
| Sleeve B | $X (XX%) |
| Sleeve D | $X (XX%) |
| Total Cash | $X (XX%) |
| Net Liquidation Value | $X (100%) |

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
- Includes full cash breakdown (SR / KR / DCA / Free Tactical)
- Includes Average Cost + Unrealized P/L (when available)
- Sorts holdings by sleeve
- Includes weight percentages for every sleeve
- Is the canonical AI-readable interchange format for all LIOS commands

## Design principle

This Markdown format becomes the **canonical portfolio interchange
format** for LIOS. Future commands (health-check, drift-check,
kelly-check, sleeve-d-check, frontier-scoreboard, rotation-analysis,
replacement-analysis) should accept the portfolio-print output
directly as input without requiring manual reformatting.

## Related

- See `../lios/components/02_capital_architecture.md` — sleeve definitions
- See `../lios/components/07_locked_decisions.md` — what may NOT change
- See `commands/snapshot.md` — simpler raw-to-snapshot converter (no avg cost / P/L / cash allocation prompt)
- See `commands/portfolio-los.md` — LOS-only dashboards (complementary, not overlapping)

## Discipline reminders

- This is an **export-only** command. Never execute trades.
- Cash allocation is **user-provided**, never assumed.
- If avg cost / P/L are unavailable from brokerage, omit them with
  "—" but keep column positions for parser consistency.
- The output must be immediately pasteable into any LLM as-is.
