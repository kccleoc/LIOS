> **Source:** Replaces the v1.x–v3.0 five-layer architecture. Governed by P1, P2, P6.

# The Five Sleeves — A / B / C / D / Z

**Component version:** v4.1
**Status:** Constitutional (P1, P2, P6)
**Last updated:** 2026-11-15

---

## Structure

```
NLV = A + B + C + D + Z
```

| Sleeve | Name | Holds | Risk role |
|---|---|---|---|
| **A** | Core Index | Broad-index ETFs only | Systematic beta |
| **B** | Conviction | 1–4 individual equities, equal weight at entry | Idiosyncratic alpha |
| **C** | Satellite | Discretionary rotation, winner-weighted | Active thematic |
| **D** | Duration | Bonds, > 3y duration, hedged | Ballast and ammunition |
| **Z** | Cash Equivalents | Duration ≈ 0 | Liquidity |

A + B + C = the risky block. D + Z = the defensive block.

**Every weight, cap and floor is a percentage of NLV.** No second denominator.

---

## Sleeve A — Core Index

**Target:** `state.yaml: sleeves.a.target_pct`
**Contents:** broad-index ETFs at fixed within-sleeve weights summing to 100%.

**Rules:**
- Index vehicles only. No individual equities, no thematic funds, no bonds.
- **No cash-like instrument may sit inside A.** A 0–1y Treasury fund is Z.
- Rebalance within A only on band breach.
- A is never sold to fund D or Z (P4). A may be trimmed into B or C.

---

## Sleeve B — Conviction

**Target:** `state.yaml: sleeves.b.target_pct`
**Structure:** 1–4 names, equal weight at entry.
**Cap:** `state.yaml: sleeves.b.position_cap_pct`, applied to **effective**
exposure (direct + index-embedded).

| Names | Slot | Drift band |
|---|---:|---|
| 1 | 100% | 60–100% |
| 2 | 50% | 35–65% |
| 3 | 33.3% | 20–50% |
| 4 | 25% | 15–35% |

**Selling:**
- **Trim** — when the sleeve is over its band, trim the **largest** position.
  Trims reduce concentration and are not exits.
- **Exit** — only on a thesis break. LOS < 60 opens a **30-day deep-dive review**
  which must resolve to either a documented re-score ≥ 60 citing new evidence, or
  exit. No third outcome. An incomplete review exits at day 30.

**Rules:**
- Effective exposure may not exceed the position cap (P6).
- New names require Investment Committee approval and LOS ≥ 80.
- US-situs holdings count toward the estate-tax ceiling (P6).

---

## Sleeve C — Satellite

**Target:** `state.yaml: sleeves.c.target_pct`
**Ceiling:** `state.yaml: sleeves.c.ceiling_pct`
**Structure:** winner-weighted by design. No equal-weight rule.
**Maximum names:** `state.yaml: sleeves.c.max_names`

**Selling:**
- **Trim** — above the ceiling, trim the **weakest** name by Replacement Index.
  C is a rotation book; laggards get cut, not the leaders.
- **Exit** — Replacement Index below floor, or a thesis break.
- Never average down a laggard.

**Rules:**
- Funded from inside C only. Never from A, never from B.
- Quarterly ranking against the sleeve benchmark. Bottom half for two
  consecutive quarters → cut, or carry a one-line written justification.
- All C surgery is governed by the Replacement Index.
- C receives no Top-up while at or above target.

---

## Sleeve D — Duration

**Target:** `state.yaml: sleeves.d.target_pct`
**Instrument:** duration > 3 years, hedged to the liability currency.

**Rules:**
- T-bills, money-market funds and 0–1y instruments are **not** D — they are Z.
- D is the designated source for Rebalance into underweight risky sleeves.
- D never funds B or C.
- D hedges growth and deflation shocks. **It does not hedge inflation shocks** —
  in an inflation-driven selloff duration falls alongside equities. That is
  accepted, and is the reason not to hold long duration exclusively.

---

## Sleeve Z — Cash Equivalents

**Target / floor / ceiling:** `state.yaml: sleeves.z.*`
**Contents:** duration ≈ 0 — T-bills, money market, bank cash.

**Rules:**
- Defined by risk factor, not issuer or ticker.
- Never reported as a bond sleeve or a hedge.
- The emergency fund sits **outside** NLV. Z is not an emergency fund.
- Above ceiling → redeploy to D, or to a risky sleeve if one is underweight.
- Below floor → funded first from Top-up.

---

## Constraints (P6)

| # | Constraint | Parameter |
|---|---|---|
| 1 | Single-name effective exposure | `sleeves.b.position_cap_pct` |
| 2 | Single-factor exposure | `constraints.factor_cap_pct` |
| 3 | Duration sleeve | `sleeves.d.target_pct` |
| 4 | Total risky weight | `glidepath.bands` |
| 5 | US-situs assets | `constraints.us_situs_ceiling` |
| 6 | Satellite ceiling | `sleeves.c.ceiling_pct` |

A target that violates a constraint is invalid and must be re-solved. When a
constraint and a rule conflict, **the constraint wins** (P6), and a breach is
itself a Rebalance trigger (P7).

**US-situs early warning:** stop adding at 80% of the ceiling and prefer
UCITS-domiciled instruments at all times. A fixed dollar ceiling plus a growing
portfolio means the constraint binds eventually if nothing is done about it.

---

## Cross-sleeve invariants

1. One denominator: NLV.
2. Caps are caps, not targets.
3. Only two operations move money (P3).
4. A sleeve ceiling never sits above its band and calls itself enforcement.
5. Cash is a sleeve, not a mood.

---

## Funding matrix

| From ↓ / To → | A | B | C | D | Z |
|---|---|---|---|---|---|
| Top-up | ✅ | ✅ | ✅ | ✅ | ✅ |
| Rebalance | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Prohibited** | — | — | — | **A → D/Z** | **A → D/Z** |

---

## Deprecated

| Concept | Fate |
|---|---|
| `ABC` denominator | Deleted |
| Strategic Reserve | Deleted |
| Kelly Reserve | Deleted |
| Free tactical bucket | Deleted |
| Regime index shifting sleeve targets | Deleted |

---

See also: `core/deployment.md`, `core/glidepath.md`, `core/regime-index.md`,
`constitution/constitution.md`.
