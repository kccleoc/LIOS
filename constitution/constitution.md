> **Source:** Extracted verbatim from LIOS v1.5 components. All L1-L18 are constitutional. Modifying any requires a major version bump (v2.0+).

# 07 · Locked Decisions

**Component version:** v1.3 + v1.4 RIL additions (L11–L14) + v1.5 PCI additions (L15–L18)
**Status:** Locked Decision
**Last updated:** 2026-07-27

---

## What This File Is

A consolidated reference for **every rule that is locked across LIOS versions**.
If you (or an AI) are tempted to suggest changing one of these,
stop. That suggestion requires a **major version bump** (v2.0+)
and a constitutional amendment — not a normal cycle review.

Trade recommendations built on top of these may change. The rules themselves do not.

---

## The Locked Decisions

### L1 · Five-Layer Capital Architecture
- **Statement:** Portfolio is partitioned into Sleeves A, B, C, D + Strategic Reserve. Each has distinct purpose, funding, caps.
- **Source:** `core/sleeves.md`, `core/strategic-reserve.md`
- **Modifying requires:** Major version bump (v2.0+)

### L2 · Sleeve A Targets (Permanent)
- **Statement:** Sleeve A carries a permanent target allocation composed of broad-index ETFs held at fixed within-Sleeve weights. The *form* (Sleeve A has permanent target allocation; broad-index ETFs carry fixed within-Sleeve weights summing to 100%) is constitutional. The *parameter values* (specific ETF tickers and their target weights) live in `state.yaml: sleeve_a.allocation`.
- **Source:** `core/sleeves.md` Sleeve A
- **Modifying requires:** Major version bump (form). Per-portfolio ETF and weight values may change via `state.yaml` edit without bump — they are personal parameters, not framework invariants.

### L3 · Sleeve B Assets and Caps
- **Statement:** Sleeve B carries high-conviction individual equities, each governed by a per-position NLV cap. The *form* (caps are binding — both buying-pause AND trim-trigger; cap hit means stop buying and, on continued momentum, trim to cap) is constitutional. The *parameter values* (specific ticker symbols and their cap percentages) live in `state.yaml: sleeve_b_caps`.
- **Source:** `core/sleeves.md` Sleeve B
- **Modifying requires:** Major version bump (form). Per-portfolio ticker and cap values may change via `state.yaml` edit without bump — they are personal parameters, not framework invariants.

### L4 · Kelly Model v2.0 Five-Dimension Scoring
- **Statement:** Five dimensions (Market Valuation 30% / Portfolio Valuation 25% / Macro & Liquidity 20% / Technical Conditions 15% / Opportunity Set 10%), each 0-100. Weighted total maps to Kelly Factor via the six-tier table.
- **Source:** `core/kelly.md`
- **Modifying requires:** Major version bump

### L5 · Replacement Index for All Sleeve D Decisions
- **Statement:** Three metrics (Frontier 40% / Momentum 35% / Diversification 25%), 0-100 each. Replacement requires composite superiority + ≥2 of 4 qualitative conditions.
- **Source:** `core/replacement-index.md`
- **Modifying requires:** Major version bump

### L6 · Strategic Reserve Funding and Deployment Rules
- **Statement:** SR funded only by external capital / exceptional realization / windfalls. Default injection split 30/70. Deployment only on enumerated triggers, via deployment hierarchy. IC has sole deployment authority.
- **Source:** `core/strategic-reserve.md`
- **Modifying requires:** Major version bump

### L7 · [Removed in v3.0 — was a portfolio-local, not a framework invariant]
- **Statement (pre-v3.0):** A specific ticker (the original repo's IBTA) was permanently exited — re-entry prohibited under any circumstances.
- **Removal rationale:** The "permanently exited" discipline is *portfolio-local*: every cloner may maintain their own list of names irrevocably excluded from their portfolio, but no specific ticker is excluded by *the framework*. The pre-v3.0 original L7 conflated Leo's personal exit (IBTA, a pre-v3.0 holding) with a constitutional rule. v3.0 removes L7 from the public framework and relocates the *concept* of a portfolio-local permanently-exited list to personal state (`state.yaml: sleeve_a.permanently_exited`). The L-number is preserved per AGENTS.md's no-renumber rule; future L-number additions skip L7.
- **Source (historical):** `core/sleeves.md` Sleeve A note (v1.x–v2.x)
- **Modifying requires:** Major version bump to *restore* L7 as a framework invariant (not anticipated).
- **Cloner note:** Add your own permanently-exited tickers to `state.yaml: sleeve_a.permanently_exited`. The validation engine honors the list exactly as the original L7 did — re-entry is flagged as a rule violation.

### L8 · Governance Principle ("Rules Permanent, Decisions Dynamic")
- **Statement:** Rules are permanent; decisions are dynamic. No trade recommendation is permanently embedded.
- **Source:** `constitution/governance.md`
- **Modifying requires:** Major version bump (this is the constitution itself)

### L9 · Communication Standards
- **Statement:** Conclusions first, dollar amounts always, explicit directives, Facts vs Assessments separation, Confidence %, no fluff.
- **Source:** `constitution/principles.md`
- **Modifying requires:** Major version bump

### L10 · Health Check 16-Section Fixed Order
- **Statement:** Health Check is always 16 sections in the canonical order. No section may be skipped.
- **Source:** `core/health-check.md`
- **Modifying requires:** Major version bump

---

## v1.4 Additions — Research Intelligence Layer (L11–L14)

The following locked decisions were introduced in v1.4. They
**add** to the constitution; they do not modify L1–L10.

### L11 · Research Intelligence Layer Is Canonical
- **Statement:** Every security researched under LIOS v1.4+ must receive LVS, LGS, LQS, LMS, and LOS scores using the standard output format defined in `core/research-layer.md`.
- **Source:** `core/research-layer.md`
- **Modifying requires:** Major version bump
- **Notes:** Sleeve A broad-index ETFs are exempt (LOS not applicable to index vehicles).

### L12 · LOS Interpretation Bands Are Fixed
- **Statement:** LOS bands are fixed at five tiers — Elite 90-100 / Buy 80-89 / Watch 70-79 / Hold 60-69 / Pass <60.
- **Source:** `core/research-layer.md`
- **Modifying requires:** Major version bump
- **Notes:** The actions associated with each band (e.g., "Buy on meaningful correction" for Watch) are part of this locked decision.

### L13 · LOS Does Not Replace Kelly, Sleeves, or Replacement Index
- **Statement:** LOS is the upstream research filter. Kelly still decides sizing, Sleeves still decide placement, Replacement Index still gates all Sleeve D surgery. LOS is **additive**, not substitutive.
- **Source:** `core/research-layer.md`, `core/replacement-index.md` v1.4 section
- **Modifying requires:** Major version bump
- **Notes:** A candidate with strong Frontier Score but weak LOS (e.g., LOS < 70) should NOT replace a Sleeve D incumbent without explicit IC rationale.

### L14 · LOS Default Weighting Orientation
- **Statement:** Default LOS weighting orientation is LQS-heaviest, then LGS/LVS, then LMS. The default weights themselves are: LQS 35% / LGS 25% / LVS 25% / LMS 15%. The exact weights may evolve over time as LIOS improves, but every LOS must be explainable.
    - **Source:** `core/research-layer.md`
- **Modifying requires:** Documented v1.4.x patch with rationale in CHANGELOG (does NOT require major bump — this is a calibration of an existing framework, not a constitutional change).
- **Notes:** Per-score deviations from default weights must be disclosed with one-line rationale.

---

## v1.5 Additions — Portfolio Construction Intelligence (L15–L18)

The following locked decisions were introduced in v1.5. They
**add** to the constitution; they do not modify L1–L14.

### L15 · LFS Is Canonical (Portfolio Fit Evaluation Required)
- **Statement:** Every investment that passes the LOS threshold (LOS ≥ 70) must also be evaluated for LFS (LIOS Fit Score) before capital allocation. LFS measures portfolio fit — how much does this investment improve the current LIOS portfolio? LFS is portfolio-relative and dynamic.
- **Source:** `core/execution-engine.md`
- **Modifying requires:** Major version bump
- **Notes:** Sleeve A broad-index ETFs are exempt (LFS not applicable to index vehicles). LFS is additive to LOS, Kelly, Sleeves, and Replacement Index — never a substitute.

### L16 · LOS Is Absolute, LFS Is Relative
- **Statement:** LOS evaluates the investment itself (absolute). LFS evaluates the investment against the current portfolio (relative). They measure different things. Both are required. Neither replaces the other. A change in LFS alone does NOT change LOS.
- **Source:** `core/execution-engine.md`
- **Modifying requires:** Major version bump

### L17 · LFS Cannot Rescue a Poor Investment
- **Statement:** A weak LOS cannot become a Buy simply because it diversifies the portfolio. LOS remains the primary investment gate. An investment with LOS < 70 does not advance to LFS evaluation. The Investment Decision Matrix (LOS + LFS) is binding: Low LOS + High LFS = Do Not Buy.
- **Source:** `core/execution-engine.md`
- **Modifying requires:** Major version bump
- **Notes:** Exceptions require explicit IC override documented in Health Check.

### L18 · LFS Preference Rule
- **Statement:** When LOS values are similar (within 5 points), LIOS should prefer the investment with the higher LFS. This is an official decision rule that guides capital allocation priority among LOS-qualified candidates.
- **Source:** `core/execution-engine.md`
- **Modifying requires:** Major version bump
- **Notes:** This rule applies to capital allocation priority, not to Replacement Index decisions (L5) which have their own criteria.

---

## Anti-Pattern: Modifying Locked Decisions Without Bumping

If an AI session suggests one of these changes without flagging it
as a constitutional amendment, treat that suggestion as suspect:

| Suggestion | Verdict |
|---|---|
| "Maybe we should add crypto to Sleeve A" | ❌ Reject — would require L1 + L2 amendments |
| "Trim a Sleeve B name's cap to a different value" | ❌ Reject as framework change; ✅ Allowed via `state.yaml: sleeve_b_caps` edit if it's your own portfolio (per-portfolio parameter, not framework) |
| "Use a 4-dimension Kelly model" | ❌ Reject — L4 amendment |
| "Replace the Replacement Index with gut feel" | ❌ Reject — L5 amendment |
| "Use SR to fund Sleeve D directly without trigger" | ❌ Reject — L6 amendment |
| "Re-enter a name on my permanently-exited list" | ❌ Reject — your own `state.yaml: sleeve_a.permanently_exited` is binding for your portfolio (formerly L7) |
| "Skip section 13 of Health Check this time" | ❌ Reject — L10 |
| "Past you said buy a name — so buy it regardless" | ❌ Reject — L8 (decisions dynamic) |
| "Skip LOS for Sleeve D, just use Frontier Score" | ❌ Reject — L13 (LOS additive) |
| "Add a 6th LOS band (e.g., 'Speculative' at 50-60)" | ❌ Reject — L12 (5 bands fixed) |
| "Make LMS the heaviest LOS weight because momentum is hot" | ⚠️ Allowed per-score deviation IF disclosed — but NOT a default change to L14 without changelog note |
| "Use LOS < 50 instead of < 60 as the Pass threshold" | ❌ Reject — L12 amendment |
| "Buy a low-LOS name because its LFS is high" | ❌ Reject — L17 (LFS cannot rescue poor investment) |
| "Ignore LFS, only use LOS for capital allocation" | ❌ Reject — L15 (LFS evaluation is required after LOS passes) |
| "LFS is high, skip the Replacement Index" | ❌ Reject — L5 (Replacement Index unchanged) + L15 (LFS additive, not substitutive) |
| "Use LFS to override Sleeve B caps" | ❌ Reject — L3 form (caps are binding) + L15 (LFS does not modify sleeve rules) |

---

## When Locked Decisions May Change

Locked Decisions may change **only** via:

1. Explicit IC decision to amend (rare)
2. Major version bump in `CHANGELOG.md`
3. Documentation of the rationale for the amendment
4. Migration plan for existing positions if applicable

There is no "soft amendment" pathway.

---

See also: constitution/governance.md, constitution/principles.md
