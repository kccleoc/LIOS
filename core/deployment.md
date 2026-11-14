> **Source:** Replaces `core/kelly.md` (Kelly Model v2.0, L4 — repealed in v3.0).

# Deployment — The Three Rules

**Component version:** v3.0
**Status:** Locked Decision (L20)
**Last updated:** 2026-11-14

---

## Purpose

Capital moves by exactly **three operations**. There is no fourth path.
Anything else is a rule violation.

| Rule | Job | Frequency | Sells? |
|---|---|---|---|
| **1. DCA waterfall** | Allocate new contributions | Monthly | No |
| **2. Rebalance band** | Correct structural drift | On breach + annual | Yes |
| **3. Discretionary channel** | Act on a specific dislocation | ≤1 per quarter | Yes (from D/Z) |

The pre-v3.0 Kelly Factor multiplied the monthly DCA by a 0–1 score. That made
a risk-appetite dial do the job of an allocation controller, and it ignored the
distance to target entirely. v3.0 removes it: the **gap** controls the DCA, and
the score survives only as a gauge (`core/regime-index.md`).

---

## Terminology

```
V_s   = current market value of sleeve s
T_s   = target dollars = w_s × (NLV + DCA)
G_s   = gap = T_s − V_s          (positive = underweight)
```

`G_s` and "drift" are the same number. There is no separate drift calculation.

---

## Rule 1 — DCA Waterfall

Runs monthly. Never sells. No discretionary input.

### Algorithm

```
1.  NLV' = NLV + DCA
2.  T_s  = w_s × NLV'                       for s in {A,B,C,D,Z}
3.  G_s  = T_s − V_s
4.  Apply ceilings:
        if w_C ≥ ceiling_C          → G_C = 0
        if w_B ≥ 0.12 and w_B ≥ target_B × 1.5 → G_B = 0
5.  Apply Z floor:
        if V_Z < floor_Z × NLV'     → fund Z to floor first; reduce DCA by that amount
6.  G⁺ = { max(G_s, 0) }
7.  if ΣG⁺ = 0:  DCA_s = DCA × w_s           (fully at target — split by weight)
    else:        DCA_s = DCA × G_s⁺ / ΣG⁺
```

**Note the standstill term.** Because the target grows with NLV, a sleeve at
target must still receive `w_s × DCA` just to hold its weight. A rule that
allocates only `G_s` will converge to a permanent positive gap and never close
it. This is why step 7 falls back to the weight split.

### Worked example — balanced book (synthetic)

```
NLV 1,000,000 | DCA 10,000 | targets A 67% B 8% C 5% D 15% Z 5%
Book is exactly on target.
G_A = 6,700  G_B = 800  G_C = 500  G_D = 1,500  G_Z = 500   Σ = 10,000
```

| Sleeve | Allocation |
|---|---:|
| A | 6,700 |
| B | 800 |
| C | 500 |
| D | 1,500 |
| Z | 500 |

At balance the rule **degenerates to the target split**. No tilt, no decision.

### Worked example — post-drawdown (synthetic)

```
NLV 700,000 | DCA 10,000 | A has fallen; D is overweight
G_A = +52,000   G_B = +11,000   G_C = +4,000
G_D = −48,000   G_Z = −9,000    ΣG⁺ = 67,000
```

| Sleeve | Allocation | Share of positive gap |
|---|---:|---:|
| A | 7,761 | 77.6% |
| B | 1,642 | 16.4% |
| C | 597 | 6.0% |
| D | 0 | overweight |
| Z | 0 | overweight |

The rule automatically starves the defensive block and floods the risky block
after a decline. **This is the crash response under a no-exit-rule policy.**

---

## Rule 2 — Rebalance Band

Runs on breach, plus an annual check. This is the only routine operation
that sells.

### Trigger

Fire if **any** of:
- some sleeve deviates > `constraints.rebalance_band_pct` absolute from target
- D exceeds its target by > 25% relative
- annual review date

### Action

```
1.  Identify overweight sleeves (G_s < 0) excluding Z below floor
2.  Sell overweight down to target
3.  Buy underweight sleeves pro-rata to their remaining gaps
4.  Z is never sold below its floor
```

An 8pp band on a $1M book is $80,000. Wide bands are deliberate: they prevent
churn, and they reserve selling for genuine structural breaks.

### Why this matters more than the hedge

In a severe drawdown the defensive block becomes overweight automatically.
Selling D and Z into the underweight risky sleeves is what converts a crash from
a loss into a discount on future purchases. **A portfolio with no exit rule must
have a rebalance rule**, or the crash response does not exist.

---

## Rule 3 — Discretionary Channel

The only other way capital moves. Deliberately small.

| Constraint | Value |
|---|---|
| Maximum per event | `constraints.discretionary_max_pct` of NLV (default 3%) |
| Maximum per quarter | 1 event |
| Funding source | D or Z only — **never A** |
| Eligibility | LOS ≥ 80 for the destination; BBI ≥ 25 |
| Approval | IC note, logged in the Health Check |

This exists so a genuine dislocation can be acted on without inventing a new
path for money. It is not a reserve: there is no earmarked cash waiting for it.

---

## Prohibited Operations

| Operation | Verdict |
|---|---|
| "Deploy the reserve because the market looks cheap" | ❌ No reserve exists; use Rule 2 |
| "Increase DCA because a score improved" | ❌ DCA is a contribution, not a signal |
| "Skip the DCA because valuations are high" | ❌ Rule 1 is mechanical |
| "Sell equities to raise cash for safety" | ❌ Z is a target, not a fear response |
| "Trim a winner to fund a laggard inside C" | ❌ C cuts laggards, never winners |
| "Sell B on a price drop" | ❌ B sells only on thesis break |
| "Move money A → D because D is underweight" | ❌ A never funds the defensive block |

---

## Deprecated in v3.0

| Concept | Fate |
|---|---|
| Kelly Factor × DCA | Deleted — gap controls allocation |
| Kelly Reserve | Deleted — undeployed DCA is Z, funded only because Z is underweight |
| Trigger ladder (L6) | Deleted — the rebalance band is the trigger |
| `ABC` denominator | Deleted — NLV only |

---

See also: `core/sleeves.md`, `core/regime-index.md`, `core/glidepath.md`.
