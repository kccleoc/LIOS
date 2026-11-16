# LIOS Changelog

All notable changes to the Leo Investment Operating System.
LIOS follows semver: major bumps only on Locked Decision changes.

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
