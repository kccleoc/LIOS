> **Source:** Extracted from LIOS v1.5. L1, L2, L3 are Locked Decisions.

# Five-Layer Capital Architecture — Sleeves A/B/C/D

**Status:** Locked Decision
**Last updated:** 2026-07-25 (extracted 2026-07-28 for v2.0)

---

## Overview

LIOS partitions all capital into five distinct layers. Each layer
has its own purpose, funding sources, caps, and decision rules.
Total NLV = A + B + C + D + SR.

The architecture is **fixed and not relitigated.**

---

## Sleeve A — Core Compounding

**Purpose:** Permanent, low-turnover compounding core. The bedrock.

**Targets (per-portfolio):**

| Ticker | Target | Role |
|---|---|---|
| (see `state.yaml: sleeve_a.allocation`) | (per-portfolio %) | Broad-index ETFs (e.g., all-world equity, S&P 500 concentration, short-duration Treasury yield buffer) |

The *form* of Sleeve A targets is fixed by Locked Decision L2: Sleeve A has a permanent target allocation composed of broad-index ETFs held at fixed within-Sleeve weights that sum to 100%. The *specific ETFs and weights* are portfolio-local parameters in `state.yaml: sleeve_a.allocation`. See `examples/state.example.yaml` for a fully populated synthetic demo.

**Total size target:** `state.yaml: sleeve_a.target_base_pct` (~70-75% of **Core Portfolio (NLV − D − SR)** by default). Sleeve D claims a separate `state.yaml: sleeve_d.target_pct_of_total_nlv` of total NLV; A's target is sized against the remaining base. When SR = $0 this simplifies to ~70-75% of (NLV − D).

**Hard rules:**
- `state.yaml: sleeve_a.permanently_exited` — list of tickers irrevocably excluded from this portfolio; re-entry is flagged as a rule violation (conceptually the pre-v3.0 L7 discipline, now portfolio-local)
- Sleeve A's cash-like yield buffer (whichever ETF fills that role in your `state.yaml`) is **not** a duration hedge — it is a yield-bearing cash substitute
- Rebalancing toward targets is permitted but not mandatory on small drift
- Never sell A to fund SR, D, or Kelly Reserve (see SR funding rules)

---

## Sleeve B — Proven Alpha

**Purpose:** High-conviction individual equities where the edge is clear.

**Constituents & caps (% of NLV):**

| Ticker | Cap | Notes |
|---|---|---|
| (see `state.yaml: sleeve_b_caps`) | (per-portfolio %) | Cap = both buying-pause AND trim trigger |

The *form* of Sleeve B caps is fixed by Locked Decision L3: each Sleeve B holding carries a per-position NLV cap that is binding (both buying-pause AND trim-trigger). The *specific ticker+cap values* are portfolio-local parameters in `state.yaml: sleeve_b_caps`. See `examples/state.example.yaml` for a fully populated synthetic demo.

**Hard rules:**
- Caps are **binding**, not advisory
- A cap hit means: **stop buying**. If momentum continues, **trim** to cap
- Names assigned to Sleeve D in your `state.yaml` stay in Sleeve D, not B, even if conviction is high (Sleeve B entry is rare and requires explicit Investment Committee approval)
- New Sleeve B names require explicit Investment Committee approval — entry is rare

---

## Sleeve C — Tactical Cash

**Purpose:** Short-horizon tactical deployments + Kelly Reserve parking.

**Target:** `state.yaml: sleeve_c.target_pct_of_abc` (~5% of (A + B + C) base by default).

**Contents:**
- **Kelly Reserve** — undeployed portion of monthly DCA (`state.yaml: monthly_dca`)
- **Short-term tactical deployments** — opportunistic, must justify expected return vs Kelly Reserve

**Hard rules:**
- Kelly Reserve grows when Kelly Factor < 1.00
- Kelly Reserve depletes via:
  - DCA deployment on Kelly Factor increase
  - Trigger-driven opportunities (see Trigger Check)
- Tactical deployments in C require explicit expected-return rationale

---

## Sleeve D — Frontier Innovation

**Purpose:** Smaller, higher-uncertainty positions in frontier tech themes.

**Target:** `state.yaml: sleeve_d.target_pct_of_total_nlv` (5% of **total NLV (including D)** by default).

**Funding sources (strict — funding source is verified at every entry):**
- ✅ External / injected capital only
- ❌ NOT from DCA
- ❌ NOT from Sleeve A sales
- ❌ NOT from Kelly Reserve
- ❌ NOT from Sleeve B trimming (unless IC-approved exception)
- ❌ NOT from SR (SR may fund D expansion, but only via deployment hierarchy)

**Staging rules (in-progress names move through stages):**
- **Stage 1 — Watchlist:** Thesis + initial score logged. No capital deployed.
- **Stage 2 — Pilot:** Small starter position (typically ≤ 0.5% NLV) — confirms thesis via price action + earnings.
- **Stage 3 — Full position:** Up to D's per-name allocation cap.

**All D surgery (entry, exit, replacement) is governed by the Replacement Index** — see `core/replacement-index.md`.

**Current constituents (per-portfolio, from `state.yaml`):**

| Ticker | Stage | Theme |
|---|---|---|
| (see `state.yaml: positions` filtered to `sleeve: D`) | (1, 2, or 3 per position) | (free-form per-position note) |

See `examples/state.example.yaml` for a synthetic populated Sleeve D, and `examples/snapshots/2026-01-01_portfolio.md` for what the generated Sleeve D snapshot block looks like.

---

## Cross-Sleeve Invariants

These rules apply across the entire architecture:

1. **No sleeve may cannibalize another's funding source.**
2. **Caps are caps** — not targets. Hitting a cap is a stop signal.
3. **SR is offensive, not defensive.** Holding cash "for safety" is anti-LIOS.
4. **Sleeve D is the only sleeve that takes new ideas.** Everything else is established.
5. **Every recommendation respects the funding-source matrix** above.

---

See also: core/strategic-reserve.md, core/kelly.md
