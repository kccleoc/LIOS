# 07 · Locked Decisions — Index

> **This file is a pointer.** The authoritative Locked Decisions live in
> `constitution/constitution.md`. Do not duplicate the rule text here —
> duplicated rules drift. Use this file only as a routing index.

**Component version:** v3.0
**Last updated:** 2026-11-14

---

| # | Status | Rule |
|---|---|---|
| **L1** | Live (amended v3.0) | Five sleeves A/B/C/D/Z; one denominator NLV |
| **L2** | Live | Sleeve A has a permanent target allocation of broad-index ETFs at fixed within-sleeve weights |
| **L3** | Live | Sleeve B per-position caps are binding (buying pause + trim trigger) |
| **L4** | **Repealed v3.0** | Kelly Model v2.0 — number gapped |
| **L5** | Live | Replacement Index governs all Sleeve C surgery |
| **L6** | **Repealed v3.0** | Strategic Reserve — number gapped |
| **L7** | **Removed v3.0** | Portfolio-local permanently-exited list |
| **L8** | Live | Governance principle — rules permanent, decisions dynamic |
| **L9** | Live | Communication standards |
| **L10** | Live | Health Check is 16 sections, fixed order |
| **L11** | Live | Research Intelligence Layer is canonical |
| **L12** | Live | LOS bands fixed at five tiers |
| **L13** | Live (reworded v3.0) | LOS does not replace sleeves, LFS or the Replacement Index |
| **L14** | Live | LOS default weighting LQS 35 / LGS 25 / LVS 25 / LMS 15 |
| **L15** | Live | LFS is canonical after LOS ≥ 70 |
| **L16** | Live | LOS absolute, LFS relative |
| **L17** | Live | LFS cannot rescue a poor investment |
| **L18** | Live | When LOS ties within 5 points, prefer higher LFS |
| **L19** | New v3.0 | One denominator — NLV |
| **L20** | New v3.0 | Three money operations only |
| **L21** | New v3.0 | BBI never changes total risky exposure |
| **L22** | New v3.0 | Sleeve B sells only on thesis break |
| **L23** | New v3.0 | US-situs estate-tax ceiling |

---

## Rules of the Rules

- L-numbers are **never renumbered**. Repealed decisions keep their number and
  are gapped.
- Modifying any live L-number requires a **major version bump** plus an ADR.
- There is no soft-amendment pathway.
- Locked Decision *parameters* (tickers, weights, caps) live in `state.yaml` and
  may be edited without a version bump. Locked Decision *forms* live in
  `constitution/` and `core/` and may not.

---

Full statements, sources and rationale: `constitution/constitution.md`.
Paste-ready summary: `lios/v3.0.md` § Locked Decisions.
