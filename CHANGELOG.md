# LIOS Changelog

All notable changes to the Leo Investment Operating System.
LIOS follows semver: major bumps only on Locked Decision changes.

---

## [3.0.0] — 2026-07-30 — Current

### Major — Open-Source Bifurcation

**Repo split.** LIOS bifurcates into three repositories:
- `kccleoc/LIOS` (public) — clean framework only; synthetic `examples/`; history from v2.0.0 boundary
- `kccleoc/LIOS-mine` (private) — Leo's live portfolio data + vendored pinned framework
- `kccleoc/LIOS-archive` (private, frozen) — original contaminated repo, renamed; old URL redirects
Decision: ADR-003. Proposal: RFC-002.

**Locked Decision reframes (L2, L3, L7).**
- L2 form: "Sleeve A has permanent target allocation composed of broad-index ETFs held at fixed within-Sleeve weights summing to 100%." ETF tickers + weight percentages → `state.yaml: sleeve_a.allocation`.
- L3 form: "Sleeve B per-position NLV caps are binding (both buying-pause AND trim-trigger)." Specific ticker + cap values → `state.yaml: sleeve_b_caps`.
- L7: removed from public framework. The "permanently exited" discipline is portfolio-local, not a framework invariant. Concept relocated to `state.yaml: sleeve_a.permanently_exited`. L-number stub preserved per AGENTS.md's no-renumber rule.
These reframes ride as minor changes *contingent on* the v3.0 OSS-split major. Future standalone L2/L3/L7 content amendments still require v3.0+ major bumps.

**Parameter extraction (10b full sweep).**
- Every ticker/dollar reference in framework dirs → extracted to state or replaced with synthetics
- Sleeve C/D target percentages → `state.yaml` params (P2)
- Monthly DCA amount → `state.yaml: monthly_dca`
- Sleeve D holding stages → `stage:` field per position in `state.yaml`
- All command-file example outputs converted to synthetic EXAMPLE-* tickers

**New synthetic examples.**
- `examples/state.schema.yaml` — empty contract with `# TODO` placeholders (CI grep-target for leak prevention)
- `examples/state.example.yaml` — fully synthetic populated demo ($1M NLV, EXAMPLE-A/B/D tickers)
- `examples/snapshots/2026-01-01_portfolio.md` — synthetic generated snapshot

**Infrastructure.**
- `framework/VERSION` + `bin/pull-framework.sh` (vendored-pin consumption; 13b behavior)
- ADR-003, RFC-002, updated glossary with v3.0 terms, rewritten README for OSS onboarding
- L1-L18 renumbered with L7 gapped (former IBTA exit → deleted; no renumber per AGENTS.md)
- Constitution: L2/L3 reframed; L7 stubbed "removed v3.0"; anti-pattern table genericized
- Core: sleeve targets → state refs; worked Health Check examples → synthetic
- Commands: all 26 command files ticker-neutral; snapshot/portfolio-print/live-health-check genericized
- All Locked Decision forms preserved. No investment philosophy changes. All scoring frameworks intact.

**Archived.**
- `lios/v1.3.md`, `v1.4.md`, `v1.5.md` → live only in `kccleoc/LIOS-archive` (private, frozen)
- `archive/v1.X/*` → live only in `kccleoc/LIOS-archive`
- `portfolio/*`, `reports/*`, `research/*`, `market/*`, `execution/*`, `watchlist/*`, `logs/*`, `workspace/*`, `snapshots/*`, `scores/*` (personal artifacts) → excluded from public repo; live only in private archive + Leo's private `LIOS-mine`

---

## [2.1.2] — 2026-07-29

### Patch
- **Computed NLV SSOT:** Authoritative `nlv = Σ(position.market_value) + cash.balance`. `ibkr_nlv` retained as IBKR Reported NLV (reference only; API/time lag expected). Check 9 fails only on internal `|nlv − portfolio_total| > $1.00`; IBKR gap is INFO never abort.
- **ADR-002 amendment:** Documents dual-field NLV policy.
- **State + full pipeline:** `portfolio/state/portfolio.yaml` 2026-07-29; validation PASSED (delta $0.00); regenerated reports/*, portfolio/current/*, snapshot `2026-07-29_portfolio.md`, workspace/*.
- **Docs:** `core/validation.md`, `core/event-pipeline.md`, `lios/v2.1.md`, `reports/reconciliation.md`.

## [2.1.1] — 2026-07-29

### Patch
- **Hard-fail reconciliation:** Remove soft-pass language from Check 9 / reconciliation report. Internal delta > $1.00 always aborts (no "understood and documented" override).
- Superseded operationally by 2.1.2 computed-NLV policy for IBKR lag.

---

## [2.1.0] — 2026-07-28

### Major Architecture Upgrade
- **Single Source of Truth (SSOT):** `portfolio/state/portfolio.yaml` as canonical data source — every portfolio number lives in exactly one file
- **Deterministic Report Generation:** 7 report templates under `reports/` — all derive from state, never independent data sources
- **Automatic Reconciliation Engine:** Σ positions + cash = NLV within ±$1 tolerance; abort generation on failure
- **Self-Healing Workspace Synchronization:** `workspace/today.md`, `next-actions.md`, `dashboard.md` auto-regenerate after portfolio events
- **Validation Pipeline:** 9-check auto-validation (position count, shares, avg cost, sleeve, cash, SR, KR, market value, NLV reconciliation) — any fail = abort
- **Research Database:** `research/database/` — migrated from `companies/`, enhanced v2.1 schema per company
- **Execution State Engine:** `execution/pending.yaml` — no pending action exists only in markdown
- **Market State Layer:** `market/market-state.yaml` — Kelly regime, macro, risk, volatility, opportunity score
- **Versioned Decision Records:** ADR-002 finalized (v2.1 SSOT architecture), ADR-003 template created
- **Automated Current Pointer Management:** `portfolio/current/` updated automatically after every snapshot
- **Event Pipeline:** `core/event-pipeline.md` — 15-step workflow from IBKR update to GitHub push
- **Watchlist Database:** `watchlist/` — queryable candidate tracking with research references
- **Validation Logs:** `logs/validation/` — audit trail of every validation pass/fail

### Bug Fixes
- Fix NLV reconciliation bug — never use cached NLV, always recompute from live state
- Fix stale `portfolio/current/` — now derives from `portfolio/state/` with auto-update
- Fix stale workspace — `today.md`, `dashboard.md`, `next-actions.md` declare auto-regeneration
- Prevent state divergence — any document with portfolio numbers must derive from state
- Introduce reconciliation report — `reports/reconciliation.md`
- Introduce validation log — `logs/validation/`

### Unchanged
- All 18 Locked Decisions (L1-L18) preserved verbatim
- Kelly Model v2.0 — weights, scoring, mapping table unchanged
- Sleeve A/B/C/D architecture and caps unchanged
- Strategic Reserve funding and deployment rules unchanged
- Research Intelligence Layer (LOS, LVS, LGS, LQS, LMS) unchanged
- Portfolio Construction Intelligence (LFS) unchanged
- Frontier methodology unchanged
- Replacement Index rules unchanged
- Health Check 16-section structure unchanged
- All investment rules, formulas, and logic carried forward unmodified
- All `lios/v1.X.md` files preserved as historical record
- All changes are additive — no existing file deleted

---

## [2.0.0] — 2026-07-28 — Superseded

### Major Architecture Upgrade
- Repository reorganized into modular agent-native structure
- New directories: constitution/, core/, portfolio/, research/, scores/, playbooks/, workspace/, templates/, rfcs/, decisions/, archive/
- VERSION.md (2.0.0), LICENSE (Proprietary) added
- lios/v2.0.md — new paste-ready init prompt (backward compatible with v1.5)
- ADR-001 — architectural decision record for v2.0
- RFC-001 — v2.0 upgrade proposal (Accepted)
- All investment rules unchanged (L1-L18 preserved verbatim)
- All 27 existing commands remain valid at original paths
- Full backward compatibility with LIOS v1.5

### Unchanged
- No investment philosophy, Kelly logic, Sleeve architecture, SR rules, RIL methodology, Health Check methodology, Replacement Index, or Constitution modified
- This upgrade changes HOW LIOS is organized — not WHAT LIOS believes.

---

## [1.5.0] — 2026-07-27 — Superseded

### Added
- **Portfolio Construction Intelligence (PCI)** — new framework for evaluating portfolio fit
- **LFS — LIOS Fit Score** — new core metric measuring how much an investment improves the current portfolio
- Five LFS dimensions: Diversification Value, Strategic Fit, Overlap, Conviction Compatibility, Portfolio Balance
- LFS Scale (90-100 Exceptional / 80-89 Strong Fit / 70-79 Useful / 60-69 Neutral / <60 Poor Fit)
- Investment Decision Matrix (LOS + LFS → Highest priority / Lower priority / Do not buy / Reject)
- Economic Engine Principle — diversification measured by underlying economic drivers, not sector labels
- LFS Constitution (5 rules) — including L18 Preference Rule (when LOS similar, prefer higher LFS)
- 8 new LFS-specific commands (see `commands/lfs*.md`, `commands/portfolio-*.md`, `commands/economic-engine-map.md`, etc.)
- New component: `09_portfolio_construction_intelligence.md`
- LFS integration into Health Check sections 5, 6, 8, 10, 11, 15

### Locked Decisions added (L15-L18)
- **L15** — LFS is canonical (portfolio fit evaluation required after LOS passes)
- **L16** — LOS is absolute, LFS is relative (both required, neither replaces the other)
- **L17** — LFS cannot rescue a poor investment (LOS remains primary gate)
- **L18** — LFS preference rule (when LOS similar, prefer higher LFS)

### Changed
- `06_health_check_structure.md` — LFS added alongside LOS in sections 5, 6, 8, 10, 11, 15; v1.5 example tables added
- `07_locked_decisions.md` — added L15-L18; anti-patterns table extended with LFS violations
- `README.md` — structure tree updated for v1.5
- `commands/README.md` — v1.5 LFS commands added to reference table
- `docs/glossary.md` — LFS terms added

### Unchanged (v1.4 carried forward, L1-L14)
- Five-layer capital architecture (A/B/C/D/SR)
- Sleeve A targets (VWRA 50 / CSPX 30 / IB01 20)
- Sleeve B caps (NVDA 20% / MU 5% / GOOG 6%)
- Kelly Model v2.0 five-dimension scoring
- Replacement Index for Sleeve D
- Strategic Reserve funding/deployment rules
- IBTA permanently exited
- Governance principle
- Communication standards
- Health Check 16-section fixed order
- Research Intelligence Layer (LVS/LGS/LQS/LMS/LOS) — fully intact
- LOS interpretation bands and default weighting

---

## [1.4.1] — 2026-07-26 — Superseded

### Added
- **`portfolio-print` command** — new canonical portfolio export format
  with full cash breakdown (SR / KR / DCA / Free Tactical), average cost,
  and unrealized P/L. Additive — no existing rules, commands, or locked
  decisions modified.

---

## [1.4.0] — 2026-07-25 — Superseded

### Added
- **Research Intelligence Layer (RIL)** — new universal scoring framework
- Five new scores: **LVS** (Value), **LGS** (Growth), **LQS** (Quality), **LMS** (Momentum), **LOS** (Overall)
- LOS interpretation bands — Elite / Buy / Watch / Hold / Pass
- Standard research output format for any company research
- Candidate research workflow (Discover → LOS → Watchlist/Deep Research → Kelly → Execution)
- 9 new LOS-specific commands (see `commands/los*.md` and `commands/portfolio-los.md`)
- LOS integration into Health Check sections 5, 6, 8, 11
- New component: `08_research_intelligence_layer.md`

### Locked Decisions added (L11–L14)
- **L11** — Research Intelligence Layer is canonical (every security scored)
- **L12** — LOS interpretation bands are fixed (5 tiers)
- **L13** — LOS does not replace Kelly, Sleeves, or Replacement Index (additive)
- **L14** — LOS default weighting orientation (LQS 35 / LGS 25 / LVS 25 / LMS 15) with disclosed deviations allowed

### Changed
- `04_replacement_index.md` — added v1.4 section showing LOS as complementary layer
- `06_health_check_structure.md` — sections 5, 6, 8, 11 now include LOS; example tables added
- `07_locked_decisions.md` — added L11–L14; anti-patterns table extended
- Portfolio snapshot — Gross Position Value corrected to $192,006 (was $191,989 in v1.3 input — minor rounding reconciliation); NLV $202,012 unchanged

### Unchanged (v1.3 carried forward, L1–L10)
- Five-layer capital architecture (A/B/C/D/SR)
- Sleeve A targets (VWRA 50 / CSPX 30 / IB01 20)
- Sleeve B caps (NVDA 20% / MU 5% / GOOG 6%)
- Kelly Model v2.0 five-dimension scoring
- Replacement Index for Sleeve D
- Strategic Reserve funding/deployment rules
- IBTA permanently exited
- "Rules permanent, decisions dynamic" governance principle
- Communication standards
- Health Check 16-section fixed order

---

## [1.3.0] — 2026-07-25 — Superseded

### Notes
- First repo-versioned release
- Initial repository structure with modular components
- Seven quick commands under `commands/`
- Date-stamped snapshot folder under `snapshots/`
- Glossary under `docs/glossary.md`
- Locked Decisions L1–L10 established (see `07_locked_decisions.md`)
- Portfolio Snapshot 2026-07-25 — NLV $202,012; Gross Position $191,989; COHR/MRVL exited via Replacement Index surgery

---

## [1.2.0] — earlier

### Notes
- Pre-repo version. Existed only as inline paste prompts.
- Governance Principle ("Rules permanent, decisions dynamic") introduced
- Kelly Model v2.0 (five-dimension) introduced
- Sleeve D staged entry / exit rules established

---

## [1.1.0] — earlier

### Notes
- Pre-repo version.
- Five-layer capital architecture established (A/B/C/D + SR)
- Initial Replacement Index (three-metric) defined
- IBTA permanently exited