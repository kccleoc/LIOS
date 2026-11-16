> **Source:** Replaces the v4.1 eight-rule constitution (five sleeves). Consolidated in v5.0.0.

# Portfolio Rules — P1′ to P8′

**Component version:** v5
**Status:** Constitutional. Changing any P-rule requires a major version bump.
**Last updated:** 2026-11-15

---

## What this file is

Eight rules. That is the whole constitution.

v4.1 answered "what may I own?" by instrument type. v5 answers it by risk role and
allows any liquid instrument that expresses the role. The change is deliberate: a
single company can fail in a way a basket cannot, and the operator's risk appetite
— total risky weight, contribution rate, age glidepath — did not change.

A rule belongs here only if breaking it would damage the portfolio. Everything
else is method.

---

## The Eight Rules

### P1′ · Four sleeves, one denominator

The portfolio is partitioned into three sleeves:

| Sleeve | Holds | Risk role |
|---|---|---|
| **A** | ETFs — broad-index **and** sector/thematic | Systematic and thematic beta |
| **B** | Satellites — individual companies and narrow bets | Idiosyncratic alpha, **cap-only** |
| **C** | Everything defensive — cash, money market, bills, bond ETFs of any duration | Ballast and ammunition, **residual** |

Every weight, cap, floor and ceiling is a percentage of **NLV**. No second
denominator exists.

**A + B is the risky block. C is the defensive block, and it is the residual:**
whatever risky does not take. One target, one arithmetic, nothing to disagree
with.

"Conviction" in Sleeve A means a documented, reviewable view expressed through a
basket — not confidence in one company. A sector or thematic ETF is a legitimate
A holding provided it carries a written view statement, a horizon, and an
invalidation condition.

**C is defined by risk role, not by duration.** Cash, bills and bonds all belong
in C, because none of them is equity beta and all of them settle in a day. The
old split between a bond sleeve and a cash sleeve was a second target that
generated defects without changing behaviour, and it is deleted. What replaces it
is one measurement rather than two sleeves: the **duration band** in P6′.

A cash-like instrument may not sit inside A. A duration-bearing instrument may
not sit inside B.

---

### P2′ · Targets come from the glidepath; risky weight is set by age

Sleeve targets are read from the age band in `state.yaml: glidepath`.

**Total risky weight is set by age — not by opinion, not by the regime index, not
by a market view.** Risky weight means `(A + B) / NLV`.

An investor who wants to change the equity level changes the glidepath band and
writes down why. Nothing else may move it.

Sleeve B carries a **ceiling, not a target**. It is excluded from the glidepath's
target arithmetic: the glidepath sets A + B, and B's ceiling constrains how much
of that total may sit in satellites.

**C is the residual**, `C = NLV − A − B`. There is no separate defensive target.
The glidepath decides one number — how much is risky — and C is whatever is left,
which makes it arithmetically impossible for the defensive targets to disagree.

---

### P3′ · Money moves two ways only

| Operation | Job | Frequency | Sells? |
|---|---|---|---|
| **Top-up** | Allocate the monthly contribution | Monthly | **No** |
| **Rebalance** | Correct a band or cap breach | Quarterly, or on a hard-cap breach | **Yes** |

There is no third operation. No discretionary channel, no reserve, no trigger
ladder, no override.

**Soft limits** — sleeve bands and the Sleeve B ceiling — are reviewed quarterly
and rebalanced only when the deviation exceeds the tolerance band in
`state.yaml: rebalance_tolerance_pct`. A deviation inside the tolerance is
reported, not traded: a rebalance cannot strike targets exactly, and a rule that
demands an exact strike will be ignored.

**Hard caps** — look-through single-company, single-sector, US-situs, and a single
defensive holding above its duration cap — fire immediately as a Rebalance trigger
(P7′).

**Escalation is symmetric and time-bounded.** A deviation beyond 3× tolerance, in either
direction, is hard. The two directions are not equal, and the asymmetry is deliberate: an
*undershoot* of risky weight has a permitted cure (C → A), so it triggers a real
Rebalance; an *overshoot* has none (A → C is prohibited), so it is reported and closed by
Top-up, never by a forced sale.

**A mandate that expires without curing its breach does not lapse silently.** On the
sunset date every uncured breach becomes hard, and the Rebalance that follows must
execute a named action. Where no permitted direction exists, the Investment Committee
must decide — in writing, within 10 business days — between amending the constitution and
accepting the breach on the record. There is no third outcome, and "wait" is not one.

A time-boxed **transition mandate** may exist (P3a) to migrate the portfolio from
one structure to another. It has a sunset date and cannot exceed the caps in P6′.

---

### P4′ · Rebalance may not de-risk the core

Rebalance may sell:

| Direction | Permitted? |
|---|---|
| Defensive → risky (C → A) | ✅ The crash response |
| Risky → risky (B → A) | ✅ Fixes concentration |
| Rotation **inside** C | ✅ Not a transfer — the duration band is cured here |
| Sale **inside** C, pro-rata | ✅ preserves the band; shortest-first would spike the average |
| **A → C** | ❌ **Prohibited** — that is de-risking |

Selling core equity to buy bonds or cash is de-risking, and de-risking is what the
glidepath decides — not a rebalance. Under a hard glidepath this restriction is
lifted, and `state.yaml` says so explicitly.

**A → B is permitted only as an Investment Committee decision** under P6′, never
as an automatic Rebalance output, because B has no target and therefore no gap to
fund.

---

### P5′ · Exits follow the instrument, not the price

**Sleeve A (ETF) holdings.** An A holding is not sold because a constituent
company's thesis breaks, because its price falls, or because a valuation
conclusion is drawn about one name inside it. A single-company event inside a fund
triggers a **look-through review** of concentration, not a sale of the fund.

An A holding is reviewed — and resized or exited — when:

1. the written view statement that justified it is invalidated;
2. a look-through cap in P6′ is breached and cannot be cured by trimming another
   A holding;
3. the fund's mandate, benchmark, index methodology, domicile, or liquidity
   changes materially; or
4. the fund's expense ratio rises materially against peers.

**Sleeve B (satellite) holdings.** A B holding is exited — taken to zero — only
when its thesis breaks. Price decline, drawdown, valuation and macro conditions
are never exit triggers.

**LOS < 60 opens a 30-day deep-dive review.** The review must resolve to either a
documented re-score of 60 or above citing new evidence, or exit. No third outcome.
If the review is not completed within 30 days, the position exits.

**A weight trim is not an exit.** Trimming is risk control and is always
permitted.

**Replacing direct single-name exposure with a fund holding the same exposure is
not an exit.** The factor weight is unchanged; only the wrapper changes. This is
the operation v5 exists to permit, and it is bounded by the Transition Mandate
rules in P3a.

---

### P6′ · Caps outrank every rule; concentration is measured through the wrapper

| Constraint | Default |
|---|---|
| **Look-through single-company exposure** (direct + fund look-through + index-embedded) | 5% of NLV |
| **Single-sector exposure** (including sector weight inside broad-index funds) | 25% of NLV |
| Sleeve B ceiling | 10% of NLV |
| Sleeve C — **average** effective duration | 1–6 years |
| Sleeve C — **any single holding** | ≤ 10 years |
| US-situs assets (non-US investor) | below the jurisdiction's estate-tax exemption |

When a cap and any other rule disagree, **the cap wins**. A target that violates a
cap is invalid and must be re-solved.

**Look-through is mandatory.** A fund position is not a single exposure for cap
purposes: it is the sum of its underlying holdings. Two funds that both hold the
same company are one exposure. Broad-index funds are included — index-embedded
weight counts.

**"Sector" is measured on a declared basis, not an assumed one.** The 25% cap is
computed on **GICS industry group** — the level at which a thematic book actually
concentrates — and the basis is recorded in `state.yaml: constraints.sector_basis`.
A cap whose basis is not recorded cannot be computed, and a book that cannot be
measured is treated as non-compliant. Broad GICS *sector* exposure (eleven
sectors) is reported as a **dashboard metric, not a cap**: a portfolio that takes
most of its risk through broad-index funds cannot satisfy an eleven-sector cap,
and a cap that cannot be satisfied is not a control — it is a rule everyone
learns to ignore. The same word must not mean two things in two reports.

**The stop-adding line binds the Top-up, not just the trade.** When US-situs
exposure is at or above 80% of the ceiling, the A share of a contribution is
directed only to A's non-US-domiciled lines; where A has none to buy, that share
goes to C. A rule that restrains deliberate trades but not the monthly
waterfall is a rule breached by the calendar.

**Duration is the one control inside C, and it replaces the old split.** With cash
and bonds in a single sleeve, the sleeve's risk is no longer implied by its name:
"defensive" describes the role, not the measurement. Average effective duration of
C must sit between **1 and 6 years**, and no single holding may exceed **10 years**.
The average is a style guard, tested at the quarterly review as its own unit — not
against a percentage-point tolerance, because years are not percentage points. The
single-holding cap is a hard breach. Measure from fact-sheet effective duration,
weighted by market value, and report **money duration in dollars** beside the
average: an average alone hides how much is actually at stake.

The lower bound exists so the sleeve is not a parked cash balance earning nothing;
the upper bound exists so a labelling decision cannot become a duration bet. A
barbell of bills and one long bond can satisfy the average while behaving in two
directions at once — the single-holding cap is what bounds it, which is why both
numbers are required.

**The sector basis is locked, not recorded.** Changing `constraints.sector_basis` is a
constitutional change — a version bump and an ADR — because a cap whose basis can be
redefined can be satisfied without trading. Re-stating the basis also re-states every
prior quarter on the record.

**Caps are point tests with a measurement buffer.** Look-through and sector weights are
summed by hand from published fund holdings, which lag and truncate. Assets are therefore
maintained at least **0.5pp inside** a cap; a measured value between cap − 0.5pp and the
cap is reported as `AT CAP — no further addition`, not as passing with room.

**`names_max` is a forward cap, and it is hard.** No *new* name may take the count above
the limit; existing names are not force-sold to satisfy a count. Selling a dust position to
reach five is compliance theatre, and the risk a name count guards against — sprawl — is
stopped by refusing the next addition. The count is reported every cycle. (v6.0.1 made this
retroactive for one day; a count cap that forces a forced sale is the wrong instrument, and
the position cap already bounds what a single name can become.)

**Sleeve B ceiling is soft.** It enforces at the quarterly Rebalance review within
the tolerance band. A satellite that appreciates past the ceiling is trimmed back
to it at that review; between reviews it may exceed the ceiling, and this is
reported every cycle rather than silently tolerated.

**Sleeve B receives no Top-up.** Ever. Satellites are funded only by an Investment
Committee decision from Sleeve A under P6′, and never by the monthly contribution.

**US-situs: stop adding at 80% of the ceiling — on every operation, not one.** This
binds the Top-up, the Rebalance, an IC decision and a transition mandate. Position
appreciation moves it too, so a breach is possible with no trade at all: at or above the
ceiling it is a hard breach with a named cure — rotate a US-domiciled line into the
non-US instrument holding the same exposure, inside the same sleeve. A capital-neutral
swap between two US-situs instruments changes nothing and is not restricted.

---

### P7′ · A cap breach is itself a Rebalance trigger

No separate mechanism is needed.

- **Hard cap breach** — look-through single-company, single-sector, US-situs, or a
  single C holding above 10 years — triggers Rebalance immediately.
- **Soft limit breach** — sleeve band beyond tolerance, Sleeve B ceiling beyond
  tolerance — triggers Rebalance at the next quarterly review, and is reported as
  a flag in the interim.

If a breach cannot be cured within the P4′ limits, it is escalated to the
Investment Committee as a structural problem, not silently tolerated: a written decision
within 10 business days, between amending the constitution and accepting the breach on
the record. **Reporting is not an outcome.**

---

### P8′ · Reports state dollars, confidence, and fixed order

Every report:

- leads with the conclusion
- states dollar amounts, not percentages alone
- separates facts from assessments
- carries a confidence percentage on each recommendation
- follows the **14-section** Health Check order when producing a full review

---

## Amendment

| Change | Requirement |
|---|---|
| Any P-rule | Major version bump, ADR, CHANGELOG entry |
| Anything in `constitution/method.md` | CHANGELOG entry only |
| Parameters in `state.yaml` (tickers, weights, caps, DCA, tolerance) | No bump — they are portfolio-local |

There is no soft-amendment pathway.

---

## What replaced what

| v4.1 | v5 |
|---|---|
| P1 five sleeves | **P1′ three sleeves** |
| P2 targets from glidepath | **P2′** — risky = A + B; B is cap-only |
| P3 two operations | **P3′** — plus quarterly tolerance for soft limits |
| P4 direction limits | **P4′** — C replaces D; A→B only by IC |
| P5 exits on thesis break | **P5′** — split by instrument; replacement is not an exit |
| P6 caps (single-name) | **P6′** — look-through company and sector caps |
| P7 breach is a trigger | **P7′** — hard immediate, soft quarterly |
| P8 16 sections | **P8′ 14 sections** |
| Sleeve D | **deleted** — duration role absorbed by C |

---

See also: `constitution/method.md`, `constitution/governance.md`,
`core/deployment.md`, `core/sleeves.md`.
