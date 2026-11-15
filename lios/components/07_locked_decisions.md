> **HISTORICAL — REFERENCE ONLY. Do not apply this file.**
>
> This file records the retired **L1–L23 Locked Decisions** and the v4.1 **P1–P8**
> rule set. Both are superseded. The current rules are **P1′–P8′** in
> `constitution/constitution.md`; where this file and the constitution disagree,
> the constitution wins and this file is wrong by definition.
>
> Kept for history and for reading old reports and ADRs. No live instruction in
> here is binding in v5.

# 07 · Locked Decisions — Historical Record (L1–L23, P1–P8)

**Component version:** historical (superseded by v5)
**Status:** Historical. Not constitutional, not method. Reference only.
**Last updated:** 2026-11-15

---

## What replaced this file

| Then | Now |
|---|---|
| L1–L23 Locked Decisions | Retired (v4.1.0). No L-numbers exist in v5. |
| v4.1 rules P1–P8 | Replaced by **P1′–P8′** in `constitution/constitution.md` (v5) |
| Five sleeves A/B/C/D/Z | Four sleeves A/B/C/Z (P1′) |
| 16-section Health Check | 15-section Health Check (P8′) |

The method that used to sit behind several of these decisions — LOS and LFS weights,
band thresholds, Replacement Index weights, report templates, communication style,
glossary entries — now lives in `constitution/method.md` and changes with a
CHANGELOG entry, no version bump.

---

## v4.1 rules as they stood, and where each went

| v4.1 | Text | v5 successor |
|---|---|---|
| **P1** | Five sleeves (A/B/C/D/Z), one denominator (NLV) | **P1′** four sleeves A/B/C/Z — D deleted, duration absorbed by C |
| **P2** | Targets from the glidepath; risky weight set by age | **P2′** — risky = A + B; B is cap-only, no target |
| **P3** | Two operations only — Top-up and Rebalance | **P3′** — plus quarterly tolerance for soft limits |
| **P4** | Rebalance direction: never sell A into D or Z | **P4′** — C replaces D; A→B by IC decision only |
| **P5** | Sleeve B exits only on thesis break, via a 30-day review | **P5′** — exits follow the instrument; a replacement is not an exit |
| **P6** | Caps outrank every rule | **P6′** — look-through company and sector caps |
| **P7** | A cap breach is itself a Rebalance trigger | **P7′** — hard immediate, soft quarterly |
| **P8** | Reports state dollars, confidence, fixed 16-section order | **P8′** — 15 sections |

---

## Why the L-numbers were retired

L1–L23 were retired in v4.1.0. Twelve of them were **method in constitutional
clothing** — they constrained how a report looked rather than what the portfolio
could hold. L4, L6 and L7 were already dead by then; L19 and L21 were merged or
deleted outright.

The failure mode was structural, not clerical: a document with twenty-three
"locked" items cannot be amended, so it stops being read, and rules that are not read
are not rules.

---

## How to use this file

- **Do not** cite an L-number or a v4.1 P-number in new output. Cite P1′–P8′ or a
  file in `core/`.
- **Do** use it to decode an old report, snapshot or ADR that references L5, L10 or
  the five-sleeve structure.
- If a live instruction appears to depend on something here, the instruction is
  stale. Re-derive it from `constitution/constitution.md`.

---

## Related

- `constitution/constitution.md` — the current rules, P1′–P8′
- `constitution/method.md` — LOS/LFS weights, bands, report templates
- `decisions/` — ADR-001 to ADR-006, the permanent record of these changes
- `CHANGELOG.md` — version history and supersession notes
