# LIOS Changelog

All notable changes to the Leo Investment Operating System.
LIOS follows semver: major bumps only on Locked Decision changes.

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
