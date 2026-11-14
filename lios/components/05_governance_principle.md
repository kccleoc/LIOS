> **v3.0 conformance note.** Written for LIOS v2.x. In v3.0 the sleeves are
> **A/B/C/D/Z** measured against NLV, the Kelly Factor is replaced by the
> **Bull–Bear Index**, and the Strategic Reserve is repealed. Where this file
> and `lios/v3.0.md` disagree, v3.0 wins.


# 05 · Governance Principle — "Rules Permanent, Decisions Dynamic"

**Component version:** v1.2 patch (still active in v1.3)
**Status:** Locked Decision
**Last updated:** 2026-07-25

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
| Five-layer capital architecture (A/B/C/D/SR) | Sleeves, funding sources | **No.** Requires major version bump + IC vote. |
| Sleeve A targets (form, L2) | Sleeve A holds permanent broad-index-ETF allocation with fixed within-Sleeve weights summing to 100% | **No.** (form); values for specific ETFs/weights live in `state.yaml: sleeve_a.allocation` |
| Sleeve B per-position caps (form, L3) | Each Sleeve B holding carries a binding per-position NLV cap (buying-pause + trim) | **No.** (form); ticker+cap values live in `state.yaml: sleeve_b_caps` |
| Kelly Model v2.0 five dimensions | The five dimensions and weights | **No.** |
| Replacement Index metrics | Three metrics and weights | **No.** |
| SR funding/deployment rules | Whitelist funding sources, deployment hierarchy | **No.** |
| Permanently-exited list (formerly L7; moved to state in v3.0) | `state.yaml: sleeve_a.permanently_exited` is binding for your own portfolio; re-entry is a rule violation | **No** (your own list is binding once declared) |
| Communication standards | Conclusions-first, dollar-amounts, etc. | **No.** |

### Decisions = trade calls. Always dynamic.

| Layer | Examples | May change? |
|---|---|---|
| "Hold a Sleeve B name at current weight" | A specific position recommendation | **Yes.** Re-evaluate every cycle. |
| "Trim a Sleeve B name at $X" | A specific trim recommendation | **Yes.** |
| "Deploy SR into a Sleeve B name" | A specific SR deployment | **Yes.** Subject to current triggers. |
| "Sleeve D surgery: exit `<TICKER-A>`, enter `<TICKER-B>`" | A specific surgery | **Yes.** Re-validate via Replacement Index. |
| "Kelly Factor this month = 0.50" | A specific Kelly call | **Yes.** Recalculate when conditions change. |

---

## Why This Distinction Exists

AI sessions have a failure mode: **embedding prior recommendations as if they were rules.**

Example of the failure:
> "Last cycle you said exit `<TICKER>`. `<TICKER>` is still in D. Why hasn't it been exited?"

This is wrong on two levels:
1. The recommendation was **a decision at that time**, not a rule.
2. Re-evaluating `<TICKER>` via Replacement Index against **current** conditions may produce a different answer.

### Correct framing (always)

❌ **"`<TICKER>` must be sold."** (treats decision as rule)
✅ **"`<TICKER>` replacement should be evaluated using the Replacement Index framework against current conditions."** (treats as ongoing decision)

The Investment Committee (Lc + AI) may **approve, defer, or reject** any prior recommendation if new information changes expected value.

---

## Versioning Implications

This principle is the basis for LIOS semver:

- **Minor bumps** (v1.3 → v1.4): dynamic-decision changes, snapshot updates, new commands, refined explanations of locked components. **No constitutional changes.**
- **Major bumps** (v1.x → v2.0): required when a **Locked Decision** changes. Should be rare — these are the rules.

Every version bump must be justified in `CHANGELOG.md` against this principle.

---

## What The Investment Committee IS

The Investment Committee is Lc + whichever AI session is active. Its remit:

| Power | Yes/No |
|---|---|
| Modify Locked Decisions | **No** (requires major version bump) |
| Approve/reject trade recommendations | **Yes** |
| Approve SR deployment | **Yes** (sole authority) |
| Add new Sleeve B names | **Yes** (rare) |
| Modify Kelly Factor this cycle | **Yes** |
| Run Replacement Index surgery | **Yes** (must follow framework) |
| Override a framework output with rationale | **Yes** (must document why) |

---

## What The Investment Committee IS NOT

- A committee that re-decides architecture every quarter.
- A place for "let's try a different allocation approach this time."
- A way to circumvent staging rules by relabeling capital.
- An excuse to skip documenting why a recommendation changed.

---

## Related
- See `07_locked_decisions.md` — the full Locked Decisions list
- See `CHANGELOG.md` — how versioning encodes this principle
- See `02_capital_architecture.md` and `03_kelly_model_v2.md` — examples of locked components