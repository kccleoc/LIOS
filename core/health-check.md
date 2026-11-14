> **Source:** Extracted from LIOS v1.5, re-pointed to the five-sleeve architecture in v3.0. L10 — Health Check 16-section fixed order is a Locked Decision.

# Health Check Structure (16 Sections)

**Component version:** v3.0
**Status:** Locked Decision (L10 — structure) / Dynamic (content per cycle)
**Last updated:** 2026-11-14

---

## Purpose

The Health Check is the **canonical output format** for a full
LIOS session review. It is the report Lc runs periodically to
re-validate the portfolio against current conditions.

**Always 16 sections, always this order.** Deviating from the order
breaks cross-session comparability.

---

## The 16 Sections

| # | Section | Output Type |
|---|---|---|
| 1 | Portfolio Health | NLV, sleeve totals vs targets, reconciliation |
| 2 | Gap & Drift | One table — gap and drift are the same number |
| 3 | Bull–Bear Index | Five scores, weighted BBI, band, band change |
| 4 | Constraint Check | Single-name, factor, duration, risky, US-situs |
| 5 | Sleeve A Review | Holdings + within-A weight drift |
| 6 | Sleeve B Review | LOS, thesis-exit status, slot drift vs band |
| 7 | Sleeve C Review | Rotation ranking, ceiling status, laggard cut list |
| 8 | Sleeve D Review | Duration, available rebalancing ammunition |
| 9 | Sleeve Z Review | Floor and ceiling, operational buffer |
| 10 | Deployment Plan | This month's Rule 1 output in dollars |
| 11 | Discretionary Channel | Open/closed, remaining budget |
| 12 | Frontier Scoreboard | Sleeve C names + candidates scored |
| 13 | Execution Calendar | Upcoming events (earnings, macro, index events) |
| 14 | Execution Plan | Concrete buy/sell/hold directives with dollar amounts |
| 15 | Risk Assessment | Constraint breaches, concentration, drawdown ladder |
| 16 | CIO Resolution | One paragraph: what to do next, with confidence % |

---

## Section Specifications

### 1. Portfolio Health
- Total NLV, gross exposure, cash, sleeve-by-sleeve breakdown
- 3-line headline: where we are, what changed since last check, what matters next
- Headline **facts**, not assessments

### 2. Drift Check
- Table: Sleeve / Target / Actual / Drift ($) / Drift (%) / Action needed
- Drift thresholds:
  - < 2% from target → no action
  - 2-5% → flag for next cycle
  - > 5% → recommend rebalance or trim
- **Always express drift in $ amounts first, % second.**

### 3. Kelly Dashboard
- Five-dimension scores + one-line rationale each
- Weighted total
- Kelly Factor
- This month's deployable DCA + Kelly Reserve impact
- See `03_kelly_model_v2.md`

### 4. Trigger Check
- Scan against SR deployment triggers:
  - S&P correction > 8-10%?
  - Nasdaq correction > 10%?
  - High-conviction unjustified selloff?
  - Earnings confirm stronger thesis while price declines?
  - Kelly Factor increase meaningful?
  - IC formal approval request?
- For each: yes/no + evidence

### 5. Sleeve A Review
- Each holding (per `state.yaml: positions` filtered to sleeve: A): shares, MV, weight, drift from `state.yaml: sleeve_a.allocation` target
- Any ticker on `state.yaml: sleeve_a.permanently_exited` referenced → flag (do not re-enter; was L7, now portfolio-local discipline)
- Sleeve A's cash-like yield buffer (whichever ETF fills that role in your `state.yaml`) is **not** a duration hedge — reaffirm if challenged
- v1.4: Sleeve A vehicles (broad-index ETFs) are not LOS-scored — note this explicitly
- v1.5: Sleeve A vehicles are also not LFS-scored (not applicable to index vehicles)

### 6. Sleeve B Review
- Each holding (per `state.yaml: positions` filtered to sleeve: B): shares, MV, % NLV, vs `state.yaml: sleeve_b_caps` for that ticker
- Cap hit = buying pause + trim signal
- Conviction commentary (thesis still intact?)
- v1.4: **Each holding shows LOS beside the position** — see example below
- v1.4: Any Sleeve B holding with LOS < 70 → flag as Pass; existing holders monitor
- v1.4: Sleeve B addition requires LOS ≥ 80 (per IC discipline; not a hard rule but expected)
- v1.5: **Each holding also shows LFS beside LOS** — see example below
- v1.5: When LOS values are similar across holdings, prioritize the holding with higher LFS (L18 preference rule)

### 7. Sleeve C Review
- Kelly Reserve balance
- Tactical deployments (if any): thesis, expected return, time horizon
- C target (~5% of A+B+C base) — drift check

### 8. Sleeve D Review
- Each holding: ticker, shares, MV, % NLV, stage (1/2/3)
- Replacement Index composite per holding
- Pending watchlist names (Stage 1)
- v1.4: **Each holding shows both Replacement Index AND LOS** — see example below
- v1.4: Any holding with LOS < 70 → flag as candidate for Replacement Index surgery
- v1.5: **Each holding also shows LFS** alongside LOS — see example below
- v1.5: When evaluating replacement candidates with similar LOS + Replacement Index, prefer the candidate with higher LFS

### 9. Strategic Reserve Review
- SR balance
- Last deployment (if any) + result
- Current deployment opportunity set (qualitative)

### 10. Strategic Reserve Opportunity Ranking
- **Required table format:**

| Rank | Asset | Expected Return | Kelly Edge | Valuation | Diversification Benefit | LOS | LFS | Action |

- Each opportunity shows LOS and LFS alongside traditional metrics
- v1.5: When opportunities have similar expected returns, prefer the one with higher LFS (L18 preference rule)
- **Guides but does NOT auto-trigger purchases.** Final call = IC.

### 11. Frontier Scoreboard
- All current D holdings + any watchlist / candidate names
- Three-metric breakdown (Frontier / Momentum / Diversification)
- Composite Replacement Index
- v1.4: **Each name also shows LOS** alongside the Replacement Index composite
- v1.5: **Each name also shows LFS** alongside LOS
- Surfaces candidates for next cycle's review

### 12. Technology Wave Map
- Active tech waves (AI infrastructure, cybersecurity, energy, robotics, quantum, memory, etc.)
- Lc's exposure to each wave (via Sleeve B + D holdings)
- Concentration vs gap analysis

### 13. Execution Calendar
- Upcoming earnings dates for Sleeve B / D holdings
- Macro events (Fed, CPI, GDP) in next 30 days
- Any IBKR-specific events (ex-dividend, corporate actions)

### 14. Execution Plan
- **The most important section.** Concrete directives:
  - "Buy $X of TICKER" with limit price + rationale
  - "Hold TICKER" with monitoring conditions
  - "Reduce $X of TICKER" with trigger condition
  - "Exit TICKER entirely" with rationale
- Each directive: Confidence %, $ amount, one-line rationale

### 15. Risk Assessment
- Concentration: top 3 holdings as % NLV
- Correlation: any concerning clustering (e.g., AI infrastructure exposure in both B and D)
- Economic Engine concentration: map holdings by economic engine (v1.5) — flag if any single engine exceeds 40% of portfolio
- Portfolio Construction Intelligence (v1.5):
  - Low-LFS names that could be rotation candidates
  - Economic engine gaps (engines with zero exposure)
  - Overlap risks (multiple holdings on same engine)
- Scenario stress:
  - What if your largest Sleeve B name −20%?
  - What if S&P -10%?
  - What if rates +100bp?
- Position-level stop-loss / rebalance triggers

### 16. CIO Resolution
- **One paragraph.** Distilled answer to: "What should Lc do next?"
- Conclusions-first. No hedging. Dollar amounts.
- This is the section Lc reads first.

---

## Cross-Section Discipline

- **Facts vs Assessments** must be separated in every section that contains both
- **Confidence %** on every recommendation in sections 5, 6, 8, 14, 15
- **Dollar amounts always** — never percentages alone
- No section may be skipped. If a section has no news, state "no action" with one-line rationale.

---

## v1.4 Example — LOS Beside Holdings

> **Note:** The example tables below use the synthetic `EXAMPLE-*` tickers
> from `examples/state.example.yaml` and `examples/snapshots/2026-01-01_portfolio.md`.
> Real users should expect their holdings to appear in these tables.
> Confidence percentages are illustrative.

### Section 6 (Sleeve B Review) — example output (synthetic data)

| Ticker | Shares | MV | % NLV | Cap | LOS | LVS | LGS | LQS | LMS | Recommendation |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---|
| EXAMPLE-B1 | 1,000.00 | $180,000.00 | 18.00% | 20% | 94 | 60 | 95 | 92 | 88 | Hold (under cap, conviction intact) |
| EXAMPLE-B2 | 200.00 | $100,000.00 | 17.00% | 5% | 87 | 78 | 90 | 85 | 75 | **Trim-trigger** (over 5% cap) |
| EXAMPLE-B3 | 400.00 | $100,000.00 | 12.00% | 6% | 91 | 72 | 88 | 94 | 80 | **Trim-trigger** (over 6% cap) |

Confidence: 80%+ on each.

### Section 8 (Sleeve D Review) — example output (synthetic data)

| Ticker | Shares | MV | % NLV | Stage | Replacement Index | LOS | Frontier | Momentum | Diversification | Verdict |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---|
| EXAMPLE-D1 | 100.00 | $12,000.00 | 1.20% | 3 | 66.5 | 82 | 75 | 65 | 55 | Hold |
| EXAMPLE-D2 | 200.00 | $12,000.00 | 1.20% | 3 | 69.3 | 88 | 70 | 75 | 60 | Hold |
| EXAMPLE-D3 | 300.00 | $10,500.00 | 1.05% | 2 | 55.0 | 74 | 60 | 55 | 45 | Watch (LOS near threshold; still in pilot) |

Confidence: 70%+ on each.

### Section 11 (Frontier Scoreboard) — example output

Same as Section 8 plus any Stage 1 watchlist names with their LOS and Replacement Index.

---

## v1.5 Example — LFS Added Beside LOS

### Section 6 (Sleeve B Review) — v1.5 output (synthetic data)

| Ticker | Shares | MV | % NLV | Cap | LOS | LFS | LVS | LGS | LQS | LMS | Decision Matrix | Recommendation |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---|
| EXAMPLE-B1 | 1,000.00 | $180,000.00 | 18.00% | 20% | 94 | 75 | 60 | 95 | 92 | 88 | Elite + Strong Fit | Hold (under cap, conviction intact) |
| EXAMPLE-B2 | 200.00 | $100,000.00 | 17.00% | 5% | 87 | 82 | 78 | 90 | 85 | 75 | Buy + Strong Fit | **Trim to 5% cap** |
| EXAMPLE-B3 | 400.00 | $100,000.00 | 12.00% | 6% | 91 | 70 | 72 | 88 | 94 | 80 | Elite + Useful | **Trim to 6% cap** |

Confidence: 80%+ on each.

### Section 8 (Sleeve D Review) — v1.5 output (synthetic data)

| Ticker | Shares | MV | % NLV | Stage | RI | LOS | LFS | Decision Matrix | Verdict |
|---|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---|
| EXAMPLE-D1 | 100.00 | $12,000.00 | 1.20% | 3 | 66.5 | 82 | 78 | Buy + Strong Fit | Hold |
| EXAMPLE-D2 | 200.00 | $12,000.00 | 1.20% | 3 | 69.3 | 88 | 85 | Buy + Strong Fit | Hold (best portfolio fit in D) |
| EXAMPLE-D3 | 300.00 | $10,500.00 | 1.05% | 2 | 55.0 | 74 | 62 | Watch + Neutral | Evaluate replacement (LFS below 65) |

Confidence: 70%+ on each.

---

## Related
- See `commands/health-check.md` — invoke this structure on demand
- See `01_identity_and_communication.md` — communication standards applied throughout
- See `07_locked_decisions.md` — section 14 directives must respect locked decisions