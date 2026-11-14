> **Source:** Replaces `core/kelly.md` (Kelly Model v2.0, repealed) and the v3.0 "three rules".

# Deployment — Top-up and Rebalance

**Component version:** v4.1
**Status:** Constitutional (P3, P4, P7)
**Last updated:** 2026-11-15

---

## Purpose

Capital moves by exactly **two operations**, plus a time-boxed transition mandate
when the structure itself is changing.

| Operation | Job | Frequency | Sells? |
|---|---|---|---|
| **Top-up** | Allocate the monthly contribution | Monthly | **No** |
| **Rebalance** | Correct a band or cap breach | On breach | **Yes** |
| Transition mandate | Migrate to a new structure | Once, sunsets | Yes |

There is no third path. No discretionary channel, no reserve, no trigger ladder.

---

## Precedence

When two rules could both act:

```
1  Caps (P6)                 — absolute; nothing overrides a cap
2  Rebalance (P3)            — overrides Top-up in any month both would act,
                               but only to the extent it actually executes a trade
3  Top-up (P3)               — the default monthly action
4  Transition mandate (P3a)  — bounded by the caps; cannot raise risky above P2
```

A Rebalance trigger that executes no trade does **not** block Top-up. That
distinction matters: without it, a stuck sleeve freezes contributions forever.

---

## Per-sleeve bands

One absolute band cannot serve sleeves of 4% and 70%. Bands scale:

```
band_s = clamp(0.5 × target_s, 2pp, 8pp)
```

| Sleeve | Target | Band | Upper edge | Lower edge |
|---|---:|---:|---:|---:|
| A | 70% | ±8.0pp | 78.0% | 62.0% |
| B | 6% | ±3.0pp | 9.0% | 3.0% |
| C | 4% | ±2.0pp | 6.0% | 2.0% |
| D | 15% | ±7.5pp | 22.5% | 7.5% |
| Z | 5% | ±2.5pp | 7.5% | 2.5% |

**A sleeve ceiling always binds before its band** if it is lower. C's ceiling is
7% but its band tops at 6%, so 6% is the effective trigger. Do not write a
ceiling above a band and call it enforcement.

---

## Top-up

Runs monthly. Never sells. No discretion.

```
NLV' = NLV + DCA
T_s  = w_s × NLV'
G_s  = T_s − V_s                     positive = underweight
G+   = max(G_s, 0)

risky_now = (A + B + C) / NLV

if risky_now >= risky_cap:
        allocate the whole DCA to D and Z, split by their target weights
else if sum(G+) = 0:
        allocate by target weight w_s
else:
        allocate DCA × G+ / sum(G+)
```

At balance this degenerates to the target split: no tilt, no decision.

**Why the standstill term matters.** The target grows with NLV, so a sleeve at
target must still receive `w_s × DCA` just to hold its weight. A rule that
allocates only the gap converges to a permanent positive gap and never closes it.
The `sum(G+) = 0` branch is the expression of that.

**The risky-cap fallback** is what keeps Top-up from ever breaching P2. When the
risky block is already at the age-band ceiling, every new dollar goes to D and Z.

---

## Rebalance

Runs on a band breach or a cap breach (P7). This is the only routine operation
that sells.

### Trigger

Any of:
- a sleeve outside its band
- a cap breach: single name, factor, duration, US-situs, sleeve ceiling
- an age-band change, after a 12-month grace period (see `core/glidepath.md`)

### Direction limits (P4)

| Sell → buy | Permitted |
|---|---|
| D or Z → A, B, C | ✅ the crash response |
| Z → D | ✅ fixes a cash overweight |
| B or C → A | ✅ fixes concentration |
| A → D or Z | ❌ **prohibited** — that is de-risking, which the glidepath decides |

Under a *hard* glidepath this restriction is lifted. `state.yaml: glidepath.policy`
says which applies.

### Action

```
1  Identify overweight sleeves and cap-breached names
2  Sell the smallest set that cures the breach, subject to P4
3  Buy underweight sleeves pro-rata to their remaining gaps
4  Z is never sold below its floor
```

### Trim selection

When a sleeve is over its band, *which* name gets trimmed:

| Sleeve | Trim |
|---|---|
| **B** | the **largest** position — trims reduce concentration |
| **C** | the **weakest** position by Replacement Index — C is winner-weighted |
| A | the largest within-A holding, back to its within-A weight |
| D / Z | no intra-sleeve selection needed; sell pro-rata |

Trimming the largest in B and the weakest in C is deliberate: B is a conviction
book where size should follow conviction, and C is a rotation book where laggards
get cut.

**A trim is not an exit.** Exits are governed by P5.

---

## Transition mandate (P3a)

A one-time structural migration is permitted, with four requirements:

1. **A sunset date.** Hard. After it, P1–P7 apply unchanged.
2. **Completion criteria.** Per-sleeve target ranges, in dollars.
3. **A fallback.** At sunset, any unmet gap is closed by Rebalance — planning
   does not continue indefinitely because a target was missed.
4. **Cap compliance throughout.** The mandate cannot raise total risky weight
   above the P2 ceiling, and cannot breach any P6 cap.

Without requirement 3, a transition quietly becomes a permanent exception. That
is how the previous architecture accumulated its exemptions.

---

## Prohibited

| Operation | Verdict |
|---|---|
| "Deploy cash because the market looks cheap" | ❌ no such operation |
| "Skip this month's contribution" | ❌ Top-up is mechanical |
| "Increase the contribution because a score improved" | ❌ contribution is a contribution |
| "Sell equities to raise cash for safety" | ❌ P4 |
| "Trim a winner to fund a laggard in C" | ❌ C cuts laggards |
| "Sell B on a price drop" | ❌ P5 |
| "Move A → D because D is underweight" | ❌ P4 |
| "One more exception, just this quarter" | ❌ the transition mandate is the only exception, and it sunsets |

---

## Deprecated

| Concept | Fate |
|---|---|
| Kelly Factor × DCA | Deleted |
| Kelly Reserve | Deleted |
| Strategic Reserve and trigger ladder | Deleted |
| Rule 3 discretionary channel | Deleted |
| `ABC` denominator | Deleted |
| Single absolute 8pp band | Replaced by per-sleeve bands |

---

See also: `constitution/constitution.md`, `core/sleeves.md`,
`core/glidepath.md`, `core/regime-index.md`.
