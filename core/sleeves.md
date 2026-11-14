> **Source:** Replaces the v1.x–v2.x five-layer architecture (Sleeves A/B/C/D + Strategic Reserve). L1 is a Locked Decision, amended in LIOS v3.0.

# Five-Sleeve Capital Architecture — A / B / C / D / Z

**Component version:** v3.0
**Status:** Locked Decision (L1)
**Last updated:** 2026-11-14

---

## Overview

LIOS partitions all capital into five sleeves. Every sleeve is measured
against **one denominator: NLV**. There is no secondary denominator.

```
NLV = A + B + C + D + Z
```

| Sleeve | Name | Risk role |
|---|---|---|
| **A** | Core Index | Systematic market beta |
| **B** | Conviction | Idiosyncratic alpha, capped |
| **C** | Satellite | Discretionary rotation, capped |
| **D** | Duration | Ballast + rebalancing ammunition |
| **Z** | Cash Equivalents | Liquidity + operational buffer |

A, B and C form the **risky block**. D and Z form the **defensive block**.

The architecture is fixed. Target percentages are parameters in `state.yaml`.

---

## Why one denominator

The pre-v3.0 architecture measured Sleeve A and Sleeve C against
`ABC = NLV − D − SR`, while measuring Sleeve B caps against NLV. A single
dollar therefore carried two different weights depending on which rule was
applied to it. The result was a gap that widened when it was funded and could
not be closed by any deployment rule.

v3.0 deletes ABC. Every sleeve, cap, floor and ceiling is a percentage of NLV.
Drift and gap are the same number.

---

## Sleeve A — Core Index

**Purpose:** Permanent, low-turnover compounding core.

**Target:** `state.yaml: sleeves.a.target_pct`
**Contents:** Broad-index ETFs only, at fixed within-sleeve weights summing to
100% (`state.yaml: sleeves.a.allocation`).

**Hard rules:**
- Index vehicles only. No individual equities, no thematic funds, no bonds.
- No cash-like instrument may sit inside A. Cash belongs in Z.
- Rebalance toward within-A weights only on band breach (`core/deployment.md`).
- A is never sold to fund B, C, D or Z.

---

## Sleeve B — Conviction

**Purpose:** High-conviction individual equities where the edge is explicit.

**Target:** `state.yaml: sleeves.b.target_pct`
**Structure:** 1–4 names, equal weight at entry.
**Cap:** `state.yaml: sleeves.b.position_cap_pct`, applied to **effective**
exposure (direct holding + index-embedded weight).

| Names | Slot weight | Drift band |
|---|---:|---|
| 1 | 100% | 60–100% |
| 2 | 50% | 35–65% |
| 3 | 33.3% | 20–50% |
| 4 | 25% | 15–35% |

**Hard rules:**
- Equal weight at entry. Names may drift inside their band; rebalance only at
  band breach, and at most annually.
- **The only sell trigger is a thesis break** — LOS < 60 or an equivalent
  research failure. Never a price move, never a stop-loss.
- Effective exposure may not exceed the position cap.
- New names require explicit IC approval. Entry is rare.
- US-situs holdings count toward the estate-tax ceiling (L23).

---

## Sleeve C — Satellite

**Purpose:** Discretionary rotation, thesis expression, higher-uncertainty
positions.

**Target:** `state.yaml: sleeves.c.target_pct`
**Ceiling:** `state.yaml: sleeves.c.ceiling_pct`
**Structure:** Winner-weighted by design. **No equal-weight rule.**
**Maximum names:** `state.yaml: sleeves.c.max_names`

**Hard rules:**
- C is funded from inside C. Never from A. Never from B.
- Winners run; laggards are cut, never averaged down.
- Quarterly ranking against the sleeve benchmark. A name in the bottom half for
  two consecutive quarters is either cut or carries a one-line written
  justification.
- Any name with LOS < 50, or a broken thesis, exits.
- Above ceiling → trim the **weakest** name, not the strongest.
- C receives no DCA while at or above target.

---

## Sleeve D — Duration

**Purpose:** The only sleeve that reliably appreciates when equities fall.
Two jobs: ballast, and rebalancing ammunition.

**Target:** `state.yaml: sleeves.d.target_pct`
**Instrument:** A duration-bearing bond vehicle — duration > 3 years, hedged to
the investor's liability currency (`state.yaml: sleeves.d.instrument`).

**Hard rules:**
- Duration > 3 years. T-bills, money-market funds and 0–1y instruments are **not**
  D — they are Z.
- D is the designated source for rebalancing into underweight sleeves.
- D is the designated source for the discretionary deployment channel.
- D never funds B or C.
- In an inflation shock D can fall alongside equities. This is accepted: D
  hedges growth shocks, not every shock.

---

## Sleeve Z — Cash Equivalents

**Purpose:** Operational liquidity and a small tactical buffer. Always the
last sleeve.

**Target / floor / ceiling:** `state.yaml: sleeves.z.*`
**Contents:** Duration ≈ 0. T-bills, money-market funds, 0–1y Treasury ETFs,
bank cash.

**Hard rules:**
- Z is defined by **risk factor, not issuer or ticker: duration ≈ 0**.
- Z is never reported as a bond sleeve, a hedge, or a duration position.
- The emergency fund sits **outside** NLV. Z is not an emergency fund.
- Z above ceiling → redeploy to the most underweight sleeve.
- Z below floor → funded first from incoming DCA.

---

## Constraint Set

Targets are outputs of constraints, not free parameters.

| # | Constraint | Parameter |
|---|---|---|
| 1 | Single-name effective exposure | `sleeves.b.position_cap_pct` |
| 2 | Single-factor exposure | `constraints.factor_cap_pct` |
| 3 | Duration hedge | `sleeves.d.target_pct` |
| 4 | Total risky weight | `constraints.risky_min_pct` / `risky_max_pct` |
| 5 | US-situs assets | `constraints.us_situs_ceiling` |
| 6 | Satellite ceiling | `sleeves.c.ceiling_pct` |

A target that violates a constraint is invalid and must be re-solved.

---

## Cross-Sleeve Invariants

1. **One denominator: NLV.** No sleeve uses a different base.
2. **Caps are caps, not targets.** A cap hit is a stop signal.
3. **No sleeve cannibalises another's designated source** (see funding matrix).
4. **Only three operations may move money** (L20 — `core/deployment.md`).
5. **Cash is not safety.** Z exists because liquidity and duration are different
   jobs, never because holding cash "avoids losses."

---

## Funding Matrix

| From ↓ / To → | A | B | C | D | Z |
|---|---|---|---|---|---|
| Monthly DCA | ✅ | ✅ | ✅ | ✅ | ✅ |
| Rebalance | ✅ | ✅ | ✅ | ✅ | ✅ |
| Discretionary channel | ❌ | ✅ | ✅ | ❌ | ❌ |

Anything not marked is prohibited without an explicit constitutional amendment.

---

## Deprecated in v3.0

| Concept | Fate |
|---|---|
| `ABC` denominator | Deleted — NLV only |
| Strategic Reserve (L6) | Repealed — dry powder is D's drift |
| Kelly Reserve | Deleted — undeployed DCA is Z |
| Free tactical (TC) bucket | Deleted — folded into Z and the discretionary channel |
| Kelly Factor (0–1) | Deleted — replaced by the Bull–Bear Index |

---

See also: `core/deployment.md`, `core/regime-index.md`, `core/glidepath.md`,
`constitution/constitution.md`.
