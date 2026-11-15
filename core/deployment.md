> **Source:** Replaces the v4.1 two-operation engine. Bands, tolerance and cap-only B are v5.

# Deployment — Top-up and Rebalance

**Component version:** v5
**Status:** Constitutional (P3′, P4′, P7′)
**Last updated:** 2026-11-15

---

## Purpose

Capital moves by exactly **two operations**, plus a time-boxed transition mandate
when the structure itself is changing.

| Operation | Job | Frequency | Sells? |
|---|---|---|---|
| **Top-up** | Allocate the monthly contribution | Monthly | **No** |
| **Rebalance** | Correct a band or cap breach | Quarterly, or on a hard-cap breach | **Yes** |
| Transition mandate | Migrate to a new structure | Once, sunsets | Yes |

There is no third path. No discretionary channel, no reserve, no trigger ladder.

---

## Precedence

When two rules could both act:

```
1  Hard caps (P6′)            — absolute; nothing overrides a cap
2  Rebalance (P3′)            — overrides Top-up in any month both would act,
                                but only to the extent it actually executes a trade
3  Top-up (P3′)               — the default monthly action
4  Transition mandate (P3a)   — bounded by the caps; cannot raise risky above P2′
```

A Rebalance trigger that executes no trade does **not** block Top-up. Without that
distinction, a stuck sleeve freezes contributions forever.

---

## Per-sleeve bands

Bands scale with the sleeve:

```
band_s = clamp(0.5 × target_s, 2pp, 8pp)
```

| Sleeve | Target | Band | Upper edge | Lower edge |
|---|---:|---:|---:|---:|
| A | per state | ±8.0pp max | — | — |
| B | **cap-only** | ceiling ± tolerance | ceiling | floor 0 |
| C | 15% | ±7.5pp | 22.5% | 7.5% |
| Z | 5% | ±2.5pp | 7.5% | floor 3% |

**Sleeve B has no band.** It has a ceiling. A sleeve with no target cannot drift
from a target; it can only breach a ceiling.

**Tolerance.** Soft limits — sleeve bands and the B ceiling — are tested against
`state.yaml: rebalance_tolerance_pct` (default 2.5pp). A deviation inside the
tolerance is **reported, not traded**. This exists because a rebalance cannot
strike targets exactly, and a rule that demands an exact strike gets ignored.

---

## Top-up

Runs monthly. Never sells. No discretion.

```
NLV' = NLV + DCA
T_s  = w_s × NLV'  for s in {A, C, Z}        ← B is excluded: no target
G_s  = T_s − V_s                              positive = underweight
G+   = max(G_s, 0)

risky_now = (A + B) / NLV                     ← C is bonds, NOT risky

if risky_now >= risky_cap:
        allocate the whole DCA to C and Z, split by their target weights
else if sum(G+) = 0:
        allocate by target weight w_s
else:
        allocate DCA × G+ / sum(G+)
```

**Three things that must not be got wrong.**

1. **`risky_now = (A + B) / NLV`.** In v4.1 this was `(A + B + C)`. C was a
   satellite then; it is the bond sleeve now. Counting it as risky reports a
   portfolio that is far more aggressive than it is, trips the risky-cap branch
   permanently, and routes every future contribution to bonds. That is a
   silent conversion of the portfolio into a bond fund.

2. **B is excluded from the target set.** B has no `w_s`, so it cannot receive a
   share of the contribution. This is the mechanical expression of "B is never
   topped up."

3. **The risky-cap fallback targets C and Z.** Not "D and Z" — D does not exist.

**Fourth gate — the US-situs stop-adding line.** The formula above allocates to
*sleeves*. Inside A, an allocated dollar buys A's lines pro-rata to their within-A
targets. Before that happens, test `state.yaml: constraints.us_situs_exposure`
against its stop-adding line. At or above 80% of the ceiling, A's share is directed
only to A's **non-US-domiciled** lines; where A has none to buy, that share goes to
C and Z instead. This exists because a US-domiciled line inside A will otherwise be
bought every month by the calendar, and a hard cap will be crossed by accumulation
rather than by any decision. The Top-up is the only operation that runs without a
trigger, so it is the only one that can breach a cap silently.

At balance this degenerates to the target split: no tilt, no decision.

**Why the standstill term matters.** The target grows with NLV, so a sleeve at
target must still receive `w_s × DCA` just to hold its weight. A rule that
allocates only the gap converges to a permanent positive gap and never closes it.
The `sum(G+) = 0` branch expresses that.

---

## Rebalance

Runs on a hard-cap breach immediately, or at the quarterly review otherwise. This
is the only routine operation that sells.

### Trigger

**Hard — immediate:**
- look-through single-company exposure above cap
- single-sector exposure above cap
- US-situs above the ceiling
- Z below its floor

**Soft — quarterly review, beyond tolerance:**
- a sleeve outside its band
- Sleeve B above its ceiling
- Sleeve B above `sleeves.b.names_max`, or a B name above `position_cap_pct`
- an A holding carrying a 0% target and still holding value (the dust rule)
- an age-band change, after the 12-month grace period

### Direction limits (P4′)

| Sell → buy | Permitted |
|---|---|
| C or Z → A, B | ✅ the crash response |
| Z → C | ✅ fixes a cash overweight |
| B → A | ✅ fixes concentration |
| A → B | ✅ **IC decision only**, never automatic |
| A → C or Z | ❌ **prohibited** — that is de-risking |

Under a *hard* glidepath the A → C/Z restriction is lifted. `state.yaml:
glidepath.policy` says which applies.

### Action

```
1  Identify hard-cap breaches, then soft breaches beyond tolerance
2  Sell the smallest set that cures the breach, subject to P4′
3  Buy underweight sleeves pro-rata to their remaining gaps
4  B is bought only under an explicit IC decision, within `names_max` and
   `position_cap_pct` — a decision that breaches either is not an IC decision,
   it is a cap breach
5  Z is never sold below its floor
6  Disposition any A holding carrying a 0% target (dust rule); proceeds to the
   most underweight A line
```

### Trim selection

| Sleeve | Trim |
|---|---|
| **A** | the largest within-A holding, back to its within-A weight |
| **B** | the **largest** position — trims reduce concentration |
| C / Z | no intra-sleeve selection; sell pro-rata |

**A trim is not an exit.** Exits are governed by P5′.

**C is not ranked.** The v4.1 rule "trim the weakest in C by Replacement Index"
is deleted: C holds bonds, and bonds are not ranked against each other for
weakness. That judgment lives in B, applied to companies.

---

## Transition mandate (P3a)

A one-time structural migration is permitted, with four requirements:

1. **A sunset date.** Hard. After it, P1′–P7′ apply unchanged.
2. **Completion criteria.** Per-sleeve target ranges, in dollars.
3. **A fallback.** At sunset, any unmet gap is closed by Rebalance.
4. **Cap compliance throughout.** The mandate cannot raise total risky weight above
   the P2′ ceiling, and cannot breach any P6′ cap.

Without requirement 3, a transition quietly becomes a permanent exception.

**Substitution clause.** Replacing direct single-name exposure with a fund holding
the same exposure — a capital-neutral swap — is permitted under a transition
mandate, because the factor weight is unchanged. It is **not** an exit under P5′.
The mandate must state the swap in dollars, hold risky weight constant, and pass
the look-through caps on the post-swap book.

---

## Prohibited

| Operation | Verdict |
|---|---|
| "Deploy cash because the market looks cheap" | ❌ no such operation |
| "Skip this month's contribution" | ❌ Top-up is mechanical |
| "Increase the contribution because a score improved" | ❌ contribution is a contribution |
| "Sell equities to raise cash for safety" | ❌ P4′ |
| "Top up the satellites, they're doing well" | ❌ P6′ — B is never topped up |
| "Sell a satellite on a price drop" | ❌ P5′ |
| "Move A → C because C is underweight" | ❌ P4′ |
| "Rebalance now because the gap is annoying" | ❌ soft limits wait for the quarterly review and the tolerance |
| "One more exception, just this quarter" | ❌ the transition mandate is the only exception, and it sunsets |

---

## Deprecated

| Concept | Fate |
|---|---|
| Kelly Factor × DCA | Deleted (v3.0) |
| Kelly Reserve | Deleted (v3.0) |
| Strategic Reserve and trigger ladder | Deleted (v3.0) |
| `ABC` denominator | Deleted (v3.0) |
| Single absolute 8pp band | Replaced by per-sleeve bands (v4.1) |
| **Sleeve D in the risky-cap fallback** | **Deleted (v5)** — C and Z |
| **`risky_now = (A+B+C)`** | **Deleted (v5)** — C is bonds |
| **Targets for Sleeve B** | **Deleted (v5)** — cap-only |
| **"Trim the weakest in C by Replacement Index"** | **Deleted (v5)** |

---

See also: `constitution/constitution.md`, `core/sleeves.md`,
`core/glidepath.md`, `core/regime-index.md`.
