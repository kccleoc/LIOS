> **v5 conformance note.** Written for LIOS v2.x. In v5 the portfolio has four
> sleeves — **A, B, C** — measured against NLV, Sleeve B is cap-only, the Kelly
> Model and the Strategic Reserve are repealed, and the current rules are
> **P1′–P8′**. Where this file and `constitution/constitution.md` disagree, the
> constitution wins.


# 05 · Governance Principle — "Rules Permanent, Decisions Dynamic"

**Component version:** v6
**Status:** Method — the versioning and IC discipline behind the constitution
**Last updated:** 2026-11-15

---

## The Principle

> **Rules are permanent. Decisions are dynamic.**

This is the constitutional layer of LIOS. It governs how every other
component gets used — and, critically, **what may and may not be
modified across versions**.

---

## What This Means in Practice

### Rules = the architecture. Permanent.

| Layer | Examples | May change? |
|---|---|---|
| Four-sleeve capital architecture (A/B/C) | Sleeves, funding sources, one denominator (P1′) | **No.** Major version bump + ADR. |
| Sleeve A holdings and view statements (P1′, P5′) | A holds ETFs only — broad-index and thematic — each thematic holding carrying a written view statement, horizon and invalidation condition | **No.** (form); tickers and weights live in `state.yaml: sleeves.a.*` |
| Sleeve B cap-only rule (P2′, P6′) | B has a ceiling, no target, no Top-up, ever | **No.** (form); ceiling and position cap values live in `state.yaml: sleeves.b.*` |
| Deployment: two operations (P3′, P4′) | Top-up, Rebalance, direction limits, quarterly tolerance | **No.** |
| Caps and look-through (P6′, P7′) | Company, sector, B ceiling, C duration, US-situs | **No.** |
| Regime Index dimensions and weights | The five dimensions and their weights | **No.** (form); it is a dashboard with no mechanical effect |
| Replacement Index metrics | Three metrics and weights, governing Sleeve B | **No.** (form); scores are dynamic |
| Permanently-exited list (moved to state in v3.0) | `state.yaml: sleeves.a.permanently_exited` is binding for your own portfolio; re-entry is a rule violation | **No** (your own list is binding once declared) |
| Communication standards | Conclusions-first, dollar amounts, confidence % | **No.** |

### Decisions = trade calls. Always dynamic.

| Layer | Examples | May change? |
|---|---|---|
| "Hold a Sleeve B name at current weight" | A specific position recommendation | **Yes.** Re-evaluate every cycle. |
| "Trim a Sleeve B name above the ceiling" | A specific trim recommendation | **Yes.** |
| "Approve a satellite funding from A" | A specific IC addition to B | **Yes.** Subject to the B ceiling. |
| "Sleeve B replacement: exit `<TICKER-A>`, enter `<TICKER-B>`" | A specific replacement | **Yes.** Re-validate via the Replacement Index, size-neutral. |
| "This month's Top-up allocates to A and C" | A specific allocation | **Yes.** The rule is fixed; the output follows the gaps. |
| "The Regime Index reads 62 this cycle" | A specific dashboard reading | **Yes.** It changes nothing mechanical. |

---

## Why This Distinction Exists

AI sessions have a failure mode: **embedding prior recommendations as if they were rules.**

Example of the failure:
> "Last cycle you said exit `<TICKER>`. `<TICKER>` is still in B. Why hasn't it been exited?"

This is wrong on two levels:
1. The recommendation was **a decision at that time**, not a rule.
2. Re-evaluating `<TICKER>` via the Replacement Index against **current** conditions may produce a different answer.

### Correct framing (always)

❌ **"`<TICKER>` must be sold."** (treats decision as rule)
✅ **"`<TICKER>` replacement should be evaluated using the Replacement Index framework against current conditions."** (treats as ongoing decision)

The Investment Committee may **approve, defer, or reject** any prior recommendation
if new information changes expected value.

---

## Versioning Implications

This principle is the basis for LIOS semver:

- **Minor bumps**: dynamic-decision changes, snapshot updates, new commands, refined
  explanations of locked components. **No constitutional changes.**
- **Major bumps**: required when a **P-rule** changes. Should be rare — these are the
  rules. Each one needs an ADR and a CHANGELOG entry.

Method changes — LOS and LFS weights, band thresholds, report templates,
communication style — need a CHANGELOG entry and no version bump.

Parameters in `state.yaml` — tickers, weights, caps, contribution, glidepath bands,
tolerance — are portfolio-local and need no bump.

Every version bump must be justified in `CHANGELOG.md` against this principle.

---

## What The Investment Committee IS

The Investment Committee is Lc plus whichever AI session is active. Its remit:

| Power | Yes/No |
|---|---|
| Modify a P-rule | **No** (requires major version bump and an ADR) |
| Approve, reduce or reject trade recommendations | **Yes** |
| Approve an addition to Sleeve B from Sleeve A | **Yes** (sole authority; P4′) |
| Approve a Sleeve B replacement | **Yes** (must follow the Replacement Index, size-neutral) |
| Add a new Sleeve B name | **Yes** (rare; LOS ≥ 80) |
| Resolve a cap breach that cannot be cured within P4′ | **Yes** — declared a structural problem, not a waiver |
| Authorise a transition mandate with a sunset date | **Yes** (P3a) |
| Override a framework output with rationale | **Yes** (must document why, with confidence %) |

---

## What The Investment Committee IS NOT

- A committee that re-decides architecture every quarter.
- A place for "let's try a different allocation approach this time."
- A way to circumvent the Sleeve B ceiling by relabeling capital.
- A channel for funding satellites from the monthly contribution — that is
  prohibited outright, not a matter for a vote.
- An excuse to skip documenting why a recommendation changed.

---

## Related

- `constitution/constitution.md` — the current rules, P1′–P8′
- `constitution/method.md` — weights, bands, templates (no bump required)
- `CHANGELOG.md` — how versioning encodes this principle
- `07_locked_decisions.md` — historical record of L1–L23 and the v4.1 P1–P8
- `02_capital_architecture.md` and `03_kelly_model_v2.md` — one live example (sleeves)
  and one retired one (Kelly), kept as the record of a repeal
