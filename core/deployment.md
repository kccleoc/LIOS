> **Source:** Replaces the v4.1 two-operation engine. Bands, tolerance, cap-only B and the single defensive sleeve are v6.

# Deployment — Top-up and Rebalance

**Component version:** v6
**Status:** Constitutional (P3′, P4′, P7′)
**Last updated:** 2026-11-16

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
| C | residual | ±8pp (clamped) | — | — |

**Sleeve B has no band.** It has a ceiling. A sleeve with no target cannot drift
from a target; it can only breach a ceiling.

**Risky weight has a band too.** `risky = (A + B) / NLV` is tested against the
glidepath's `risky_pct` at the same 2.5pp tolerance, at the quarterly review. A shortfall is
closed by the Top-up tilt and by nothing else — under `contribution_tilt` the tilt *is* the
instrument. Without this band the one number the whole glidepath is built on had no test at
all: every sleeve could sit inside its own band while total risk was above the age target.

**Tolerance.** Soft limits — sleeve bands, the risky band and the B ceiling — are tested
against `state.yaml: rebalance_tolerance_pct` (default 2.5pp). A deviation inside the
tolerance is **reported, not traded**. This exists because a rebalance cannot strike targets
exactly, and a rule that demands an exact strike gets ignored.

**Escalation — a soft breach is not soft forever.** A deviation beyond **3× tolerance**, or
one surviving **two consecutive quarterly reviews**, is treated as hard — unless an open
Transition Mandate cures it inside its window. Reporting is not enforcement: without this, a
chronic breach persists indefinitely as long as it is correctly reported.

**The two directions escalate differently, and the asymmetry is deliberate.** An undershoot
of risky weight has a permitted cure (C → A), so escalation produces a real trade. An
overshoot has none (A → C is prohibited), so escalation produces a report and a Top-up, not
a forced sale. Saying so plainly is better than a rule that pretends a cure exists.

**Sunset is a trigger, not an expiry.** A mandate that reaches its sunset with a breach
uncured makes that breach hard on the sunset date, and the following Rebalance must execute
a named action. Where no permitted direction exists, the IC decides in writing within 10
business days: amend the constitution, or accept the breach on the record. No third outcome,
and silence is not one.

**No borrowing, ever.** The account is margin-type so that unsettled sale proceeds can fund
a same-session buy — not so the book can be levered. Gross exposure may not exceed NLV
(`leverage ≤ 1.00×`). A buy leg is placed only after its funding sells have filled, and only
up to settled cash plus those fills; a rolled or partial buy is capped at
`min(remaining quantity, floor(available cash / limit price))`. Validation Check 12 aborts
production on negative cash. **Buying power is not capital** — it is the broker's estimate
of what could be borrowed, and nothing here borrows.

---

## Top-up

Runs monthly. Never sells. No discretion.

```
NLV' = NLV + DCA
T_s  = w_s × NLV'  for s in {A, C}           ← B is excluded: no target
                                              ← w_C = 1 − risky_target
G_s  = T_s − V_s                              positive = underweight
G+   = max(G_s, 0)

risky_now = (A + B) / NLV                     ← C is defensive, NOT risky

risky_cap = risky_pct + rebalance_tolerance_pct      (80% + 2.5pp = 82.5%)

if risky_now >= risky_cap:
        allocate the whole DCA to C
else:
        allocate DCA × G+ / sum(G+)
```

**Three things that must not be got wrong.**

1. **`risky_now = (A + B) / NLV`.** C is the defensive sleeve and is never counted as
   risky. Counting it trips the risky-cap branch permanently and routes every future
   contribution into the defensive block — a silent conversion of the portfolio, with no
   error and no flag.
2. **B is excluded from the target set.** B has no `w_s`, so it cannot receive a share of
   the contribution. That is the mechanical expression of "B is never topped up."
3. **The risky-cap fallback targets C.** It is the only defensive sleeve, so the branch
   that used to starve one while feeding the other does not exist.

**Within C, the contribution buys pro-rata to C's existing composition** (cash, if C is
empty). C has no target and no internal split, but an unallocated dollar is still a monthly
decision for the owner. Pro-rata holds the duration average where it was and keeps the
decision out of the loop. Without it, the same $1,500 can drift the sleeve in either
direction every month, and the quarterly band test becomes a verdict rather than a control.

**The fourth gate — the US-situs stop-adding line.** The formula allocates to *sleeves*.
Inside A, an allocated dollar buys A's lines pro-rata to their within-A targets. Before
that happens, test `constraints.us_situs_exposure` against its stop-adding line. At or above
80% of the ceiling, A's share is directed only to A's **non-US-domiciled** lines; where A has
none, that share goes to C. The gate binds **every** operation — Top-up, Rebalance, an IC
decision, a mandate — and appreciation moves the figure too, so a breach is possible with no
trade at all. The Top-up is the only operation that runs without a trigger, which is why it
is the one that can breach a cap silently.

**The fallback splits by gap, not by target weight.** Splitting by target weights looks tidy
and feeds whichever sleeve is already overweight. With one defensive sleeve that failure
mode is gone: the fallback is "all of it to C", and C's gap is whatever the residual says.
This branch also gives A nothing even when A is underweight, which is correct — risk is at
its cap — but must be **reported**, because an underweight sleeve receiving zero is
indistinguishable from one that was forgotten.

**The standstill branch is gone, because it could never run.** It said: "if `sum(G+) = 0`,
allocate by target weight". But `Σ(T_s − V_s)` over {A, C} is `NLV' − B − (A + C)`, which is
exactly `DCA`. The gaps therefore always sum to the contribution, and `sum(G+) = 0` requires
`DCA = 0` — in which case there is no Top-up to allocate. It was an unreachable branch
carrying an undefined weight, which is worse than dead code: it looked like the rule that
handled balance, so nothing else was written to handle balance.

The concern it was meant to answer is real, and the formula already answers it: a sleeve at
target receives `DCA × G+ / ΣG+`, non-zero exactly when the contribution is, so a sleeve at
target keeps growing with NLV. No branch is needed.

**A's gap cannot always be funded by the Top-up.** If `risky_now` is at the cap the whole
contribution goes to C, and A stays underweight until either the cap falls or a Rebalance
moves C → A. That is deliberate — but it means an underweight A while risk sits at the cap
is a **Rebalance** condition, not a Top-up condition. Report it as one, or it presents as
neglect.

**A skipped month is deferred, not cancelled.** The contribution is a fixed rate, and the
plan has no discretion to reduce it. A month that is not executed becomes arrears, repaid on
top of the normal contribution over the following months — never as a single lump that
changes the allocation arithmetic. Without this, one missed month drifts the whole book
permanently, because the gap the Top-up was closing simply stops closing.

---

## Rebalance

Runs on a hard-cap breach immediately, or at the quarterly review otherwise. This
is the only routine operation that sells. The review's dates and standing agenda are
fixed in `constitution/method.md`; nothing here depends on when the owner happens to
remember to look.

### Trigger

**Hard — immediate:**
- look-through single-company exposure above cap
- single-sector exposure above cap
- US-situs above the ceiling
- a single C holding above its duration cap
- a B name above `position_cap_pct` — the per-name 5% test. It is hard on every report,
  not merely at a review: at the default settings a B name above 5% of NLV is also a
  look-through breach, so a soft reading would have the same fact firing immediately
  under one rule and waiting a quarter under another. Listed here because P6′'s hard list
  is written in risk terms, and this is the same cap expressed per name

**Soft — quarterly review, beyond tolerance:**
- a sleeve outside its band
- Sleeve B above its ceiling
- Sleeve B above `sleeves.b.names_max` — the **count**, not the addition. The next
  addition is refused when it is proposed (P6′); a count already above the limit is
  settled at the review, and this line never authorises a forced sale to make a count
  come out
- an A holding carrying a 0% target and still holding value (the dust rule)
- an age-band change, after the 12-month grace period

### Direction limits (P4′)

| Sell → buy | Permitted |
|---|---|
| C → A | ✅ the crash response |
| B → A | ✅ fixes concentration |
| A → B | ✅ **IC decision only**, never automatic |
| inside C (any rotation) | ✅ not a transfer; cures the duration band |
| A → A (within-sleeve) | ✅ cures a look-through cap inside A; a rotation, not an exit |
| A → C | ❌ **prohibited** — that is de-risking |

Under a *hard* glidepath the A → C restriction is lifted. `state.yaml:
glidepath.policy` says which applies.

### Action

```
1  Identify hard-cap breaches, then soft breaches beyond tolerance
2  Sell the smallest set that cures the breach, subject to P4′
3  Buy underweight sleeves pro-rata to their remaining gaps
4  B is bought only under an explicit IC decision, within `names_max` and
   `position_cap_pct` — a decision that breaches either is not an IC decision,
   it is a cap breach
5  C is never sold below the money needed to keep the risky block in band, and a sale
   inside C is pro-rata across its holdings — never shortest-first
6  Disposition any A holding carrying a 0% target (dust rule); proceeds to the
   most underweight A line
```

### Trim selection

| Sleeve | Trim |
|---|---|
| **A** | the largest within-A holding, back to its within-A weight |
| **B** | the **largest** position — trims reduce concentration |
| C | no intra-sleeve selection for sizing; sell pro-rata, then re-cure duration |

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
| **Sleeve D in the risky-cap fallback** | **Deleted (v5)** — and the C split with it in v6 |
| **`risky_now = (A+B+C)`** | **Deleted in v5** — C is defensive, never risky |
| **Targets for Sleeve B** | **Deleted (v5)** — cap-only |
| **"Trim the weakest in C by Replacement Index"** | **Deleted (v5)** |

---

See also: `constitution/constitution.md`, `core/sleeves.md`,
`core/glidepath.md`, `core/regime-index.md`.
