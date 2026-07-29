# LIOS Event Pipeline
# Version: 2.1.0
#
# ⚠️  This is the official LIOS workflow. Every portfolio event follows this pipeline.
# Each step defines: trigger, read, write, and abort condition.

## Pipeline Overview

```
IBKR Update
    ↓
Update portfolio state      → portfolio/state/portfolio.yaml
    ↓
Validate (9 checks)         → core/validation.md
    ↓
Recalculate sleeves         → core/sleeves.md
    ↓
Recalculate Kelly           → core/kelly.md
    ↓
Recalculate drift           → core/sleeves.md (targets)
    ↓
Recalculate LOS/LFS         → core/research-layer.md, core/execution-engine.md
    ↓
Recalculate SR              → core/strategic-reserve.md
    ↓
Generate reports            → reports/*.md
    ↓
Update workspace            → workspace/*.md
    ↓
Archive snapshot            → portfolio/snapshots/
    ↓
Update current/             → portfolio/current/
    ↓
Update VERSION.md           → VERSION.md
    ↓
Commit changes              → git commit
    ↓
Push GitHub                 → git push
```

---

## Step Details

### Step 1: IBKR Update
- **Triggers:** User reports new IBKR data, Portfolio Print command, Health Check command
- **Reads:** IBKR statement or user-provided prices/shares/cash; IBKR Reported NLV optional
- **Writes:** Nothing yet — input only
- **Abort if:** No new data provided

### Step 2: Update Portfolio State
- **Triggers:** Any change in portfolio data (trade, price update, capital injection)
- **Reads:** Previous `portfolio/state/portfolio.yaml`
- **Writes:** Updated `portfolio/state/portfolio.yaml`
- **Must set:** `nlv = Σ(position.market_value) + cash.balance` (authoritative)
- **May set:** `ibkr_nlv` = IBKR Reported NLV (reference only)
- **Abort if:** Update would create duplicate position or invalid data

### Step 3: Validate (9 Checks)
- **Triggers:** After any state update, before any report generation
- **Reads:** `portfolio/state/portfolio.yaml`, `core/validation.md`
- **Writes:** `logs/validation/YYYY-MM-DD.md` (or timestamped)
- **Abort if:** ANY check fails — do not proceed to reports
- **Check 9:** |nlv − portfolio_total| ≤ $1.00; |ibkr_nlv − nlv| logged as INFO only

### Step 4: Recalculate Sleeves
- **Triggers:** After validation passes
- **Reads:** `portfolio/state/portfolio.yaml`, `core/sleeves.md`
- **Writes:** Sleeve totals (in-memory, used by subsequent steps)
- **Abort if:** Position missing sleeve assignment

### Step 5: Recalculate Kelly
- **Triggers:** After sleeve recalculation, or on Kelly recalc command
- **Reads:** `portfolio/state/portfolio.yaml`, `market/market-state.yaml`, `core/kelly.md`
- **Writes:** Kelly Factor (in-memory)
- **Abort if:** Missing market state data

### Step 6: Recalculate Drift
- **Triggers:** After sleeve recalculation
- **Reads:** `portfolio/state/portfolio.yaml`, `core/sleeves.md` (targets/caps)
- **Writes:** Drift values per sleeve (in-memory)
- **Abort if:** Target values undefined

### Step 7: Recalculate LOS/LFS
- **Triggers:** After Kelly recalculation, or on LOS/LFS command
- **Reads:** `research/database/*.md`, `core/research-layer.md`, `core/execution-engine.md`
- **Writes:** Updated scores in `research/database/*.md` (if changed)
- **Abort if:** Missing research data for active positions

### Step 8: Recalculate SR
- **Triggers:** After drift check, or on SR trigger evaluation
- **Reads:** `portfolio/state/portfolio.yaml`, `core/strategic-reserve.md`
- **Writes:** SR status (in-memory)
- **Abort if:** SR rules violation detected

### Step 9: Generate Reports
- **Triggers:** After all calculations complete and validation passes
- **Reads:** `portfolio/state/portfolio.yaml`, `market/market-state.yaml`, `execution/pending.yaml`, `research/database/*.md`, `watchlist/`
- **Writes:** `reports/health-check.md`, `reports/drift.md`, `reports/kelly.md`, `reports/execution.md`, `reports/sleeve-d.md`, `reports/sr.md`, `reports/reconciliation.md`
- **Abort if:** Any report template's data source is unavailable

### Step 10: Update Workspace
- **Triggers:** After report generation
- **Reads:** `portfolio/state/portfolio.yaml`, `execution/pending.yaml`, `logs/validation/` (latest)
- **Writes:** `workspace/today.md`, `workspace/next-actions.md`, `workspace/dashboard.md`
- **Abort if:** Workspace template corrupted

### Step 11: Archive Snapshot
- **Triggers:** After workspace update
- **Reads:** `portfolio/state/portfolio.yaml`
- **Writes:** `portfolio/snapshots/YYYY-MM-DD_portfolio.md`
- **Abort if:** Snapshot already exists for this date (unless intentional overwrite)

### Step 12: Update Current Pointer
- **Triggers:** After snapshot archived
- **Reads:** Latest `portfolio/snapshots/YYYY-MM-DD_portfolio.md`
- **Writes:** `portfolio/current/portfolio.md`, `portfolio/current/allocation.md`, `portfolio/current/cash.md`
- **Abort if:** Current/ files already up-to-date

### Step 13: Update VERSION.md
- **Triggers:** Only on version bumps (not on routine snapshot updates)
- **Reads:** `VERSION.md` (current), `CHANGELOG.md`
- **Writes:** `VERSION.md` (updated)
- **Abort if:** Version bump conflicts with existing version

### Step 14: Commit Changes
- **Triggers:** After all previous steps complete successfully
- **Reads:** `git status`
- **Writes:** `git add` + `git commit` (ONLY with explicit user approval per AGENTS.md)
- **Abort if:** User declines commit

### Step 15: Push GitHub
- **Triggers:** After commit succeeds
- **Reads:** `git remote`
- **Writes:** `git push`
- **Abort if:** Push rejected (conflicts, permissions)

---

## Event Types and Pipeline Entry Points

| Event | Enter at Step | Skip |
|-------|--------------|------|
| IBKR price update | 1 | — |
| Trade executed | 1 | — |
| Capital injection | 1 | — |
| Kelly recalc only | 5 | 1-4 already current |
| LOS/LFS update | 7 | 1-6 already current |
| Snapshot generation | 11 | 1-10 already current |
| Report regeneration | 9 | 1-8 already current |

---

## Agent Responsibilities

Per LIOS v2.1, agents SHALL:
- **Maintain state:** Update `portfolio/state/portfolio.yaml` as the single source of truth
- **Validate consistency:** Run the 9-check validation before any generation
- **Generate reports:** Produce all reports deterministically from state
- **Detect drift:** Flag any reconciliation delta > $1.00
- **Detect stale files:** Identify documents not regenerated after state changes
- **Suggest corrections:** Never silently overwrite state; always explain changes

---

*This pipeline is the official LIOS v2.1 workflow. All steps reference 
`portfolio/state/portfolio.yaml` as the canonical data source. No step 
independently calculates positions.*
