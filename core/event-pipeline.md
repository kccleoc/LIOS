# LIOS v4.1 Event Pipeline

**Version:** 3.0
**Last updated:** 2026-11-14

The official workflow. Every portfolio event follows this pipeline. Each step
defines what it triggers on, reads, writes, and when it aborts.

## Pipeline Overview

```
IBKR / broker update
    ↓
Update portfolio state       → portfolio/state/portfolio.yaml
    ↓
Validate (11 checks)          → core/validation.md
    ↓
Recalculate sleeves          → core/sleeves.md
    ↓
Recalculate gaps             → core/deployment.md
    ↓
Recalculate Regime Index  → core/regime-index.md
    ↓
Recalculate LOS / LFS        → core/research-layer.md, core/execution-engine.md
    ↓
Constraint check             → core/sleeves.md (constraint set)
    ↓
Deployment plan (Rules 1–2)  → core/deployment.md
    ↓
Generate reports             → reports/*.md
    ↓
Update workspace             → workspace/*.md
    ↓
Archive snapshot             → portfolio/snapshots/
    ↓
Update current/              → portfolio/current/
    ↓
Commit                       → git commit (explicit approval only)
    ↓
Push                         → git push (explicit approval only)
```

---

## Step Details

### Step 1 · Broker Update
- **Triggers:** new broker data, Portfolio Print, Health Check command
- **Reads:** broker statement or user-provided prices/shares/cash; broker NLV optional
- **Writes:** nothing — input only
- **Abort if:** no new data

### Step 2 · Update State
- **Triggers:** any change in portfolio data (trade, price, capital injection)
- **Reads:** previous `portfolio/state/portfolio.yaml`
- **Writes:** updated `portfolio/state/portfolio.yaml`
- **Must set:** `nlv = Σ(position.market_value) + cash.balance`
- **May set:** `ibkr_nlv` (reference only)
- **Abort if:** duplicate position or invalid data

### Step 3 · Validate (9 Checks)
- **Triggers:** after any state update, before any report
- **Reads:** state, `core/validation.md`
- **Writes:** `logs/validation/YYYY-MM-DD.md`
- **Abort if:** any check fails — do not proceed to reports

### Step 4 · Recalculate Sleeves
- **Triggers:** validation pass
- **Reads:** state, `core/sleeves.md`
- **Writes:** sleeve totals and weights (% of NLV), in memory
- **Abort if:** position missing a valid sleeve

### Step 5 · Recalculate Gaps
- **Triggers:** sleeve recalculation
- **Reads:** state, sleeve targets
- **Writes:** `G_s = w_s × (NLV + DCA) − V_s` per sleeve, in memory
- **Abort if:** targets undefined or do not sum to 100
- **Note:** gap and drift are the same number in v3.0. Do not compute them twice.

### Step 6 · Recalculate Regime Index
- **Triggers:** after gaps, or on `/regime-recalc`
- **Reads:** state, `market/market-state.yaml`, `core/regime-index.md`
- **Writes:** five dimension scores, weighted RI, band
- **Abort if:** market inputs missing (report the gap; do not extrapolate)
- **Note:** output is a band, never a dollar amount.

### Step 7 · Recalculate LOS / LFS
- **Triggers:** after RI, or on an LOS/LFS command
- **Reads:** `research/database/*.md`, `core/research-layer.md`,
  `core/execution-engine.md`
- **Writes:** updated scores (if changed)
- **Abort if:** an active Sleeve B or C position has no current research record
- **Note:** LOS < 60 on a Sleeve B name is a **mandatory exit** (P5), not a
  discussion item.

### Step 8 · Constraint Check
- **Triggers:** after LOS/LFS
- **Reads:** state, constraint set in `core/sleeves.md`
- **Writes:** constraint table (limit / actual / status), in memory
- **Abort if:** never — breaches are reported, not fatal
- **Note:** compute **effective** single-name exposure (direct + index-embedded).
  A direct weight inside the cap can still breach on an effective basis.

### Step 9 · Top-up and Rebalance
- **Triggers:** constraint check complete
- **Reads:** gaps, `core/deployment.md`
- **Writes:** Top-up allocation; Rebalance flag if any sleeve breaches a band or a
  cap (P7)
- **Abort if:** a data input needed for the allocation is missing
- **Note:** if Rebalance triggers **and executes a trade**, it absorbs the
  contribution and Top-up is not run separately. If it triggers but executes no
  trade, Top-up still runs.

### Step 10 · Generate Reports
- **Triggers:** all calculations complete, validation passed
- **Reads:** state, market state, `execution/pending.yaml`, research database,
  watchlist
- **Writes:** `reports/health-check.md`, `reports/drift.md`,
  `reports/deployment.md`, `reports/execution.md`, `reports/sleeve-c.md`,
  `reports/reconciliation.md`
- **Abort if:** a report's data source is unavailable
- **Note:** `reports/kelly.md` and `reports/sr.md` no longer exist.

### Step 11 · Update Workspace
- **Triggers:** reports generated
- **Reads:** state, `execution/pending.yaml`, latest validation log
- **Writes:** `workspace/today.md`, `workspace/next-actions.md`,
  `workspace/dashboard.md`
- **Abort if:** workspace template corrupted

### Step 12 · Archive Snapshot
- **Triggers:** workspace updated
- **Reads:** state
- **Writes:** `portfolio/snapshots/YYYY-MM-DD_portfolio.md`
- **Abort if:** snapshot exists for this date (unless intentional overwrite)

### Step 13 · Update Current Pointer
- **Triggers:** snapshot archived
- **Reads:** latest snapshot
- **Writes:** `portfolio/current/portfolio.md`, `allocation.md`, `cash.md`
- **Abort if:** `current/` already up to date

### Step 14 · Commit
- **Triggers:** all prior steps succeeded
- **Reads:** `git status`
- **Writes:** `git add` + `git commit` — **explicit user approval required**
- **Abort if:** user declines

### Step 15 · Push
- **Triggers:** commit succeeded and the user asked for a push
- **Reads:** `git remote`
- **Writes:** `git push`
- **Abort if:** rejected (conflict, permission)

---

## Event Types and Entry Points

| Event | Enter at | Skip |
|---|---|---|
| Broker price update | 1 | — |
| Trade executed | 1 | — |
| Capital injection | 1 | — |
| RI recalc only | 6 | 1–5 current |
| LOS / LFS update | 7 | 1–6 current |
| Snapshot generation | 12 | 1–11 current |
| Report regeneration | 10 | 1–9 current |

---

## Agent Responsibilities

Agents SHALL:

- **Maintain state:** update `portfolio/state/portfolio.yaml` as the single
  source of truth
- **Validate first:** run the 11-check validation before any generation
- **Generate deterministically:** every report derives from state
- **Compute gaps once:** gap and drift are one metric
- **Compute effective exposure:** direct plus index-embedded, always
- **Detect stale files:** identify documents not regenerated after a state change
- **Refuse the fourth path:** no money moves outside the three deployment rules
- **Never silently overwrite state:** explain every change

Agents SHALL NOT:

- invent a Kelly Factor or a reserve
- use `ABC` as a denominator
- sell a Sleeve B name on price
- change total risky exposure because of a market view

---

*This pipeline is the official LIOS v4.1 workflow. All steps reference
`portfolio/state/portfolio.yaml` as the canonical source. No step independently
calculates positions.*
