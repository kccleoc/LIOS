# LIOS Changelog

All notable changes to the Leo Investment Operating System.
LIOS follows semver: major bumps only on Locked Decision changes.

---

## [6.0.5] — 2026-11-17

### Method — the Quarterly Review has dates, an agenda, and a marks convention

P3′ and P7′ both say soft limits are reviewed quarterly, and P7′ escalates a deviation that
survives two consecutive reviews. Nothing anywhere said *when*. A deadline with no date is a
sentence, not a control: it can be read as "whenever it is convenient", and the two-review
escalation could not start counting.

`constitution/method.md` now carries a **Quarterly Review** section: the review is held on
the **last Saturday of February, May, August and November** — 27 Feb, 29 May, 28 Aug and
27 Nov 2027, then the same rule every year. It is struck on **quarter-end closing prices**
(31 Dec, 31 Mar, 30 Jun, 30 Sep) and the latest reported earnings, so the caps are tested
against an agreed number rather than against the week the review happened to be held. The
weekend matters for the same reason: by then the reporting season is closed and the market
is shut, so the review cannot move the price it is measuring.

It also records the standing agenda (recompute every cap at quarter-end prices; re-score
LOS for every B name; decide the B count against `names_max`; check the Top-up tilt), and
restates that **every decision is an IC record — no record, no funding** and that **hard
caps are never deferred to a review**.

**Why method and not the constitution.** The Amendment table makes any P-rule edit a major
bump plus an ADR. The dates change no cap, target, tolerance, sleeve or operation, so the
calendar lives where the framework already put workflow — `method.md`, "editable with a
CHANGELOG entry". The section says so explicitly and forbids carrying a rule change in with
it. Moving the dates into P3′/P7′ proper would be v7.0.0 + an ADR, and that is the owner's
call, not a side effect of this patch.

**The section is pinned.** Because it now defines what P3′ and P7′ mean by "quarterly", it
is not free method text: changing a date, the marks convention or the agenda order needs a
major version bump and an ADR, not a CHANGELOG entry. Otherwise the cheapest edit in the
system could move the meaning of a constitutional term, which is the loophole the Amendment
table exists to close.

**`names_max` re-stated as two different things.** `core/sleeves.md` and the Rebalance
trigger now separate the forward cap's two halves: the **block on the next addition is
immediate and undeferrable**, while a **count already above the limit is a soft item settled
at the review**. `core/deployment.md` listed the count under "soft — quarterly" while
`constitution/constitution.md` called the cap "hard"; both readings were defensible and
that is exactly what made the sentence dangerous. The wording now says which half is which.
The count also comes down by decision — which name, on its thesis, LOS and Replacement
Index — never by selling the smallest position to make arithmetic work.

### Validation — two checks could not pass under v6

`core/validation.md` Check 6 banned "a cash-like instrument (duration < 1y) assigned to
sleeve `a` or `c`", and tested a `sleeve z` for duration-bearing instruments. Both are v5
statements. v5's C was bonds above three years and sleeve Z held the near-cash, so the rule
was coherent then. v6 deleted Z and merged it into C (ADR-008), which inverted the first
bullet: it failed the one sleeve it was written to protect. Run literally, Check 6 fails
every v6 book and, under "never generate a report from an unvalidated state", blocks every
report. Rewritten: cash-like in C is correct by definition, duration lives only in C, and
the `sleeve z` bullet is gone. The original purpose is intact — duration is the test, not
the issuer.

### Classification — `position_cap_pct` is hard, and it now says so

`core/sleeves.md` called it "a hard test on every report", `core/research-layer.md` called
it "binding under P6′", and `core/deployment.md` listed a B name above it under "soft —
quarterly review". Three documents, two answers. Moved to the hard list, with the reason
written down: at the default settings a B name above 5% of NLV is *also* a look-through
breach, so the soft reading had the same fact firing immediately under one rule and waiting
a quarter under another. No operative outcome changes today — nothing is near it.

### Structure — the Health Check is 14 sections, and now it is numbered like it

The v6.0.0 restructure merged the two defensive reviews and left the numbering damaged in
four places: `core/health-check.md` was titled "(15 Sections)", its specs ran 1–8 then
jumped to 10 and ended at 15; `templates/HealthCheck.md` carried the same jump; the
`lios/components/06` summary said 15 in its title and 14 in its table; and
`constitution/method.md` listed fifteen items with "9. Top-up Plan" written twice.
All four now read 1–14. Cross-references were re-pointed to match: Execution Plan is §13
(was §14), Satellite Scoreboard is §11 (was §12), Top-up Plan is §8.

---

## [6.0.4] — 2026-11-16

### Documentation — v6 is the current version everywhere it is read

Live documentation still described v5 as the operating system in thirteen places: the
`Component version` header on every core file, the validation document's own title, a
15-section note in the health check that survived the move to 14, and command banners and
prompts that introduced themselves as v5.

All corrected. What was **not** touched is the history: "deleted in v5", "the v5 split",
"15 in v5", the v5 ADRs and RFCs, and the v5 CHANGELOG entry. Those are facts about a
prior version, and rewriting them would make the record untrue. The rule is: a document
may say v5 only when it is describing the past.

---

## [6.0.3] — 2026-11-16

### Patch — a name count is a forward cap, not a forced sale

v6.0.1 made `names_max` hard and retroactive, in response to a review finding that the
book's seven names were a breach the breach report omitted. Making it *hard* was right;
making it *retroactive* was not. A count cap that binds incumbents turns into a forced sale
of whichever name is smallest — in this book a $772 position — which is compliance theatre,
and the risk a name count actually guards against (sprawl) is stopped by refusing the next
addition.

`names_max` now binds additions: no new name above the limit, and no forced sale of an
existing one to make the count come out. `position_cap_pct` remains a hard per-name test on
every report, and both are reported every cycle.

---

## [6.0.2] — 2026-11-16

### Patch — a cap that forbade the simplest correct portfolio

v6.0.1 capped any single holding at 40% of Sleeve C's **money duration**, to stop an
average from hiding a large rate position. It was wrong, and the current book proves it:
the sleeve's only bond fund is 15% of C by value and 74.5% of its money duration, because
duration has to live somewhere. The cap therefore required at least three bond funds, or a
bond allocation so small it defeated the purpose — regulation by fragmentation.

Replaced with the quantity that actually separates "a bond allocation" from "a duration
bet": **holdings with duration above 6 years may not exceed 50% of C's value.** The
current book sits at 14.9%. A half-and-half barbell sits exactly at the limit, which is the
deliberate line between a sleeve that carries duration and one that is a duration bet.

The 1–6y average, the 10-year per-holding hard cap, and pro-rata sales inside C are
unchanged.

---

## [6.0.1] — 2026-11-16

### Patch — four holes an external review put a finger through, and one dead branch

Two independent reviews of v6.0.0. What follows is what survived checking: every finding
here was reproduced from the text before it was accepted.

**A branch that could never execute.** The Top-up's standstill rule said "if
`sum(G+) = 0`, allocate by target weight". But `Σ(T_s − V_s)` over {A, C} is
`NLV' − B − (A + C)`, which is exactly `DCA` — so the gaps always sum to the contribution
and the branch requires `DCA = 0`, where there is no Top-up. It was unreachable, carried an
undefined weight, and looked like the rule that handled balance, so nothing else was
written to handle it. Deleted, with the proof in its place.

**No borrowing rule, on a margin account.** The account is margin-type so that unsettled
proceeds can fund a same-session buy. Nothing said the book may not be levered. A buy leg
larger than its funding sells would have converted a paired trade into leverage, duplicated
the factor exposure and accrued interest, with no rule and no check noticing. Now: leverage
≤ 1.00×, buys only after funding sells fill, a rolled buy capped by available cash, and new
Check 12 aborts on negative cash.

**A duration band that a barbell satisfies.** Average duration plus a per-holding cap does
not bound the stake: half of C in a 10-year bond gives a 5.1y average, passes both tests,
and puts $2,444 at risk per 1pp. Holdings with duration above 6 years are now capped at 50%
of C's **value**. Sales inside C are pro-rata, because draining the bills in a crash would
spike the average through the ceiling mid-event.

**Caps wired to one operation.** The US-situs stop-adding line gated the Top-up only, while
appreciation, a Rebalance, an IC decision, a mandate and a contribution routed to C could
each add exposure. It now binds every operation, and names its cure — rotate a US-domiciled
line into the non-US instrument holding the same exposure, inside the same sleeve.

**A sector basis anyone could redefine.** Declaring the basis was a real improvement over
leaving "sector" undefined, but a basis that can be edited to make a breach disappear is not
a cap. Changing `constraints.sector_basis` now requires a version bump and an ADR.

**Also fixed:** `risky_cap` was used in the pseudocode and never defined (now
`risky_pct + tolerance`); A's target could go negative when B exceeds the risky budget (now
floored at zero and reported as an IC-level failure); `names_max` is now hard and reported
per cycle rather than only at the quarterly review; escalation is symmetric, with the
asymmetry between overshoot and undershoot stated instead of implied; a mandate that reaches
its sunset with a breach uncured now makes it hard on that date, with a 10-day written IC
deadline; caps carry a 0.5pp measurement buffer because a hand-built look-through is not a
point measurement; Check 5 now recomputes targets and gaps from holdings instead of trusting
the stored block; a contribution reaching C buys pro-rata to C's existing composition, which
was previously an unallocated monthly decision; and "closed by the Top-up tilt and by nothing
else" now says "primarily", because a Rebalance into an underweight A does close it too.

---

## [6.0.0] — 2026-11-16

### Major — one defensive sleeve, measured by duration

**Fourth sleeve deleted. Three sleeves: A, B, C. Constitution P1′ changes, so this is
a major bump, not a patch.** ADR-008, RFC-006.

#### The change

Cash, money market, T-bills and bond ETFs merge into a single defensive sleeve **C**,
which becomes the **residual**: `C = NLV − A − B`. The cash target, the 3% floor and
the 7.5% ceiling are deleted. There is no internal distribution — composition is
reported, never targeted.

One control replaces the split: **average effective duration 1–6 years, and no single
holding above 10 years.** Measured value-weighted from fact-sheet effective duration,
with money duration reported in dollars. The average is a quarterly style guard; a
single holding above 10 years is a hard breach.

#### Why

The C/Z boundary was drawn by an instrument property — duration — and used as if it
were a risk role. Everything else followed:

- One cash-like instrument was misclassified for weeks, and the error survived a
  rewrite because nothing measured it.
- A 5% target with a 2.5pp tolerance is **±50% of the sleeve**. That is not a limit;
  it is a suggestion, and it breached continuously.
- Two defensive targets could disagree — and in the shipped state they did, by
  $6,204 of de-risking.
- A whole transition mandate existed to move money between two defensive sleeves.

All constituents settle in a day, so the split preserved no liquidity advantage.

#### What this deletes

| Deleted | Replaced by |
|---|---|
| Sleeve Z and its target 5% / floor 3% / ceiling 7.5% | C, residual |
| Sleeve C's 15% duration target | C, residual |
| The cash-vs-bond classification rule | The duration band |
| `C → Z` and `Z → C` as separate directions | Rotation inside C, which is not a transfer |
| "Duration sleeve 15%" cap | "C duration 1–6y avg, none > 10y" |
| 15-section Health Check | 14 sections — the Z review merges into C |
| 10 validation checks | 11 — new Check 11, C duration band |

#### Structural simplifications

- The glidepath carries **one** number per age band (risky), plus the B ceiling. A
  and C are both residual, so three independent targets can no longer disagree.
- The Top-up formula runs over `{A, C}`. The risky-cap fallback is simply "all to C" —
  the branch that could starve one defensive sleeve while feeding the other is gone.
- `risky = (A + B) / NLV` is unchanged, and so is everything about A and B.

#### The cost, stated plainly

A sleeve with a 1-year floor must hold at least 1 year of duration, so a book sitting
in bills is out of band from adoption. That is deliberate: 0.37 years is a cash
balance earning nothing, and the floor makes the cost of that visible instead of
labelling it "defensive".

---

## [5.0.2] — 2026-11-16

Findings from an external adversarial review. Two of them are worse than anything
the pre-registered suspect list contained, and both are the same failure: a
document that describes a plan rather than executing it.

- **`validation.md` could not pass on a post-mandate state and could not run one of
  its own checks.** Checks 2, 3 and 8 fail on any position carrying zero shares —
  which is exactly how a not-yet-executed mandate target must be written. Check 7
  read `bbi.*`, a v2.1 field that no v5 state contains. A pipeline that cannot pass
  is a pipeline nobody runs, which is indistinguishable from not having one. Checks
  2/3/8 now skip `status: pending`; Check 7 became optional and dashboard-only, and
  its "second band step is a rule violation" note is deleted — the Regime Index
  gates nothing, and a dashboard cannot be violated.
- **New Check 10 · US-situs derivation.** The figure must equal the sum of the lines
  flagged US-situs plus broker cash. This is the check that would have caught the
  v5.0.1 defect before a human did.
- **A soft breach had no magnitude limit.** A sleeve could sit at six times its
  tolerance forever, correctly reported, and nothing fired. Beyond 3× tolerance, or
  surviving two consecutive reviews, a soft breach is now hard.
- **Risky weight had no band at all.** The number the entire glidepath is built on
  was never tested; every sleeve could be inside its own band while total risk was
  above the age target. It now has one, tested quarterly, closed by Top-up tilt and
  by nothing else.
- **The risky-cap fallback split by target weight, not by gap**, feeding whichever
  of C or Z was already overweight. It also gave A nothing while A was underweight,
  which is correct but must be reported.
- **Two directions were missing from the P4′ table.** A Z-floor breach triggers an
  immediate Rebalance for which no direction funded Z, and a look-through cap inside
  A had no permitted cure. `C → Z` and `A → A` are now named.
- **The grace-end de-risking rule could not be obeyed.** `glidepath.md` said a
  leftover age-step gap "is closed by Rebalance", while P4′ prohibits the only
  direction that would close it. Under `contribution_tilt` the band is a direction,
  not a deadline, and the gap is reported open every cycle.
- **A skipped contribution had no recovery rule.** One missed month drifted the book
  permanently. Arrears are now repaid at the normal rate, never as a lump.
- **`w_s` was undefined in the standstill branch**, where A's target is residual.
  An undefined weight makes a branch unexecutable, which is how rules stop running.
- **The Investment Committee had no decision test.** It is one human plus whichever
  session is open, so "an IC decision" funded the one sleeve where judgment replaces
  arithmetic. Five recorded items now constitute a decision; no record, no funding.

---

## [5.0.1] — 2026-11-16

Four defects found by recomputing the book rather than reading the rules. All four
are the same species v5 was meant to eliminate: a rule that is declared but never
enforced, or measured on a basis nobody wrote down.

- **The 25% sector cap had no taxonomy.** "Sector" was undefined, so the same book
  was compliant or in immediate hard breach depending on whether the reader meant a
  GICS sector or an industry group. Now measured on **GICS industry group**, with
  the basis recorded in `constraints.sector_basis`; broad-sector totals become a
  dashboard metric. A cap with no recorded basis is reported as `NON-COMPLIANT`,
  never as passing.
- **The US-situs stop-adding line did not bind the Top-up.** Measured correctly, a
  book can sit above 80% of the estate-tax ceiling while the monthly waterfall keeps
  buying a US-domiciled line inside A — crossing a hard cap by calendar, not by
  decision. The fourth Top-up gate now restricts A's share to non-US-domiciled
  lines while the line is live.
- **`names_max` and `position_cap_pct` were dead declarations.** Present in state,
  referenced by no trigger, gate or step. Both are now soft triggers at the
  quarterly review and hard conditions on any IC decision to fund B.
- **A holding at a 0% target kept its capital.** A residual-target formula can only
  allocate *to* A as a whole, so a position listed at `target_pct: 0` had no exit
  path at all. The dust rule dispositions it at the next Rebalance.

Also recorded: buy legs are sized on **actual net proceeds**, never on an estimate
struck three days before the fill. Sizing a buy on an estimate produces either idle
cash or an unfunded order.

---

## [5.0.0] — 2026-11-15

### Major — four sleeves, ETF-basket conviction, cap-only satellites

The constitution is rewritten. v4.1's five sleeves and its individual-equity
Sleeve B are replaced by four sleeves in which a sector conviction is expressed as
a **basket**, not a pick.

**Why.** A single-company event in a single-name sleeve is a portfolio event. When
a chief executive departs and a position falls roughly thirty percent while its
sector does not, the v4.1 answer is "review, then sell" — a rule for after the
damage. The operator asked for a structure that makes the damage smaller in the
first place, holding total risk, contribution rate and glidepath constant.

**The rules.**

| Was (v4.1) | Is (v5) |
|---|---|
| P1 five sleeves A/B/C/D/Z | **P1′ four sleeves A/B/C/Z** — D deleted |
| A = broad-index ETFs only | **A = ETFs, broad-index *and* sector/thematic** |
| B = 1–4 equities, equal weight, targeted | **B = satellites, cap-only** |
| C = satellite rotation, winner-weighted | **C = duration bonds** |
| P2 risky = A+B+C | **P2′ risky = A + B** |
| P3 rebalance on breach | **P3′ soft limits rebalance quarterly within a tolerance** |
| P4 A→D/Z prohibited | **P4′ A→C/Z prohibited; A→B only by IC decision** |
| P5 exits on thesis break | **P5′ exits follow the instrument; replacement is not an exit** |
| P6 single-name cap 10% | **P6′ look-through company cap 5%, sector cap 25%** |
| P7 breach is a trigger | **P7′ hard breaches immediate, soft breaches quarterly** |
| P8 **16** sections | **P8′ 15 sections** |

**Three defects fixed in the deployment engine.** These were live in v4.1 and would
have produced wrong output on any v5 book:

1. `risky_now` was `(A + B + C) / NLV`. C is the bond sleeve in v5, so the formula
   over-reported risky weight badly enough to trip the risky-cap branch
   permanently — routing every future contribution to bonds and never funding A
   again. Now `(A + B) / NLV`.
2. The risky-cap fallback allocated to "D and Z". D does not exist. Now C and Z.
3. "Trim the weakest in C by Replacement Index" survived into a sleeve that holds
   bonds. Bonds are not ranked for weakness. Deleted; the judgment lives in B.

**Sleeve B mechanics.** No target, no Top-up, ever. Additions are an Investment
Committee decision funded from A. Trims happen at the quarterly review when B
exceeds its ceiling beyond the tolerance band — a winner is allowed to run between
reviews, and the overage is reported every cycle rather than silently tolerated.

**Tolerance.** Soft limits are tested against `rebalance_tolerance_pct` (default
2.5pp). A deviation inside tolerance is reported, not traded. A rule that demands
an exact strike gets ignored, so the tolerance is written down instead.

**Unchanged.** Total risky weight, the age glidepath, the monthly contribution, and
the two-operation structure. This is a repackaging, not a re-risking.

**Also changed.** `core/sleeves.md`, `core/deployment.md`, `core/health-check.md`
(16→15), `core/glidepath.md`, `commands/drift-check.md`, `commands/health-check.md`.
New: `rfcs/RFC-005.md`, `decisions/ADR-007.md`.

**Migration.** `LIOS-mine/execution/2026-11-15-v5-restructure.md` — capital-neutral,
with the single-company-to-basket swap staged behind the pending earnings gate.

---

## [4.1.2] — 2026-11-15

### Patch — remove the renamed file

`framework/lios/v3.0.md` deleted. It was renamed to `lios/v4.1.md` in 4.1.1, but
`bin/push-framework.sh` copies changed files and cannot propagate deletions, so
the old path survived upstream with stale content.

The repo now carries exactly one current init prompt: `lios/v4.1.md`.

---

## [4.1.1] — 2026-11-15

### Patch — one version number for methodology and framework

`lios/v3.0.md` renamed to **`lios/v4.1.md`** (same content). From 4.1.0 onward the
methodology carries the framework release number.

---

## [4.1.0] — 2026-11-15

### Major — eight portfolio rules replace the Locked Decision layer

The constitution is reduced from twenty live Locked Decisions to eight rules.
Method moves to `constitution/method.md` and can be edited with a changelog entry.
The deployment engine reduces to two named operations.

See `decisions/ADR-006.md`.
