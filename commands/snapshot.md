> **v3.0 conformance note.** Written for LIOS v2.x. In v3.0 the sleeves are
> **A/B/C/D/Z** measured against NLV, the Kelly Factor is replaced by the
> **Bull–Bear Index**, and the Strategic Reserve is repealed. Where this file
> and `lios/v3.0.md` disagree, v3.0 wins.


# `/snapshot` — Standardize Live Portfolio Snapshot

## Purpose

Convert raw portfolio data (from IBKR, spreadsheet, etc.) into the
canonical LIOS snapshot format. Every command that takes a snapshot
as input expects this format.

## When to use

- After any trade execution (so the snapshot stays current)
- Before pasting a snapshot into any command
- Whenever Lc is unsure whether the snapshot is in the right shape

## Command

---

```
Standardize my portfolio data into the LIOS snapshot format.

Raw portfolio data:
<paste raw holdings — IBKR export, list of tickers+shares+prices,
or anything you have>

Output a snapshot table in this exact format:

| Ticker | Shares | Price | Market Value |

Then compute:
- Gross Position Value (sum of Market Values)
- Cash (from IBKR)
- Net Liquidation Value (Gross + Cash)

Then output the sleeve classification:

| Sleeve | Tickers | Sub-total |
|---|---|---|---|
| A | (from `state.yaml: positions` filtered to sleeve: A) | $X |
| B | (from `state.yaml: positions` filtered to sleeve: B) | $X |
| C | (Kelly Reserve + tactical) | $X |
| D | (from `state.yaml: positions` filtered to sleeve: D) | $X |
| SR | (cash held for deployment triggers) | $X |

Sanity checks:
- Sum of sleeves + cash = NLV
- No ticker appears in two sleeves
- `state.yaml: sleeve_a.permanently_exited` list checked — no banned tickers present
- Any unknown ticker: flag for classification

Begin.
```

---

## What you get back

A standardized snapshot table + sleeve classification + sanity
check confirmation. Drop this directly into any LIOS command.

## Example output

**Snapshot — example (synthetic data)**

| Ticker | Shares | Price | Market Value | Sleeve |
|---|---|---|---|---|
| EXAMPLE-A1 | 1,000.0000 | $100.00 | $100,000 | A |
| EXAMPLE-A2 | 500.0000 | $200.00 | $100,000 | A |
| EXAMPLE-A3 | 1,000.0000 | $100.00 | $100,000 | A |
| EXAMPLE-B1 | 1,000.0000 | $180.00 | $180,000 | B |
| EXAMPLE-B2 | 200.0000 | $500.00 | $100,000 | B |
| EXAMPLE-B3 | 400.0000 | $250.00 | $100,000 | B |
| EXAMPLE-D1 | 100.0000 | $120.00 | $12,000 | D |
| EXAMPLE-D2 | 200.0000 | $60.00 | $12,000 | D |
| EXAMPLE-D3 | 300.0000 | $35.00 | $10,500 | D |

Gross Position Value: $714,500
Cash (incl. Kelly Reserve + SR): $275,500
**Net Liquidation Value: $990,000**

Sleeve Breakdown:
- A: $300,000 (30.3% NLV)
- B: $380,000 (38.4% NLV)
- C: TBD (Kelly Reserve)
- D: $34,500 (3.5% NLV)
- SR: TBD

Sanity checks: ✅ All tickers classified. ✅ `state.yaml: sleeve_a.permanently_exited` checked — no banned tickers present. ✅ No duplicates.

## Discipline reminders

- Always include **cash**, not just positions — Kelly Reserve and SR live in cash.
- Sleeve classification must be explicit; no "leftover" holdings.
- If a holding doesn't fit any sleeve, flag for IC review (could be a new Sleeve D candidate or a misclassified asset).

## Related

- See `../snapshots/` — saved snapshots for reference
- See `commands/drift-check.md` — primary consumer of snapshots
- See `commands/health-check.md` — full Health Check expects a snapshot as input