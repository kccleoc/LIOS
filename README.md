# LIOS — Leo Investment Operating System

Institutional-grade portfolio management framework, codified.
A rules-based, AI-portable operating system for self-directed
investing — designed to be pasted into any AI chat session
and recover full context instantly.

> **Why this repo exists.** AI chats don't have long-term memory.
> Without a structured initialization prompt, every new session
> starts from zero, hallucinates portfolio state, and drifts
> from prior reasoning. LIOS solves this by making the system
> itself the source of truth — versioned, modular, paste-ready.

---

## What is LIOS?

LIOS is **not a stock-picking service**. It is:

1. A **capital architecture** — three sleeves (A/B/C) measured against one denominator, with a constraint set that outranks every rule
2. A **scoring framework** — Regime Index (5 dimensions, dashboard only) + LOS/LFS research gates + Replacement Index (3 metrics)
3. A **deployment engine** — exactly two operations: Top-up and Rebalance
4. A **report structure** — 14-section Health Check, fixed order, always
5. A **constitution** — eight rules, P1′–P8′, and nothing else

The system is executed in parallel across any LLM. Every session reads the same initialization prompt → identical framework, identical voice, identical evaluation discipline.

---

## Quick Start

| What you want to do | Go to |
|---|---|
| Initialize a new AI session | [`lios/v4.1.md`](./lios/v4.1.md) — paste the entire file |
| Set up your own portfolio | Copy [`examples/state.example.yaml`](./examples/state.example.yaml) → rename → swap tickers → save as your `state.yaml` |
| Understand the state contract | [`examples/state.schema.yaml`](./examples/state.schema.yaml) — every field with `# TODO` descriptions |
| See a demo snapshot | [`examples/snapshots/2026-01-01_portfolio.md`](./examples/snapshots/2026-01-01_portfolio.md) — synthetic example |
| Run a quick command | [`commands/README.md`](./commands/README.md) |
| Understand one concept deeply | [`lios/components/`](./lios/components/) |
| Check version history | [`CHANGELOG.md`](./CHANGELOG.md) |
| Look up a term | [`docs/glossary.md`](./docs/glossary.md) |

---

## How to Use LIOS for Your Own Portfolio

### 1. Clone the framework
```bash
git clone https://github.com/kccleoc/LIOS.git
```

### 2. Create your portfolio state
```bash
cp examples/state.example.yaml my-portfolio/state.yaml
```
Edit `my-portfolio/state.yaml` — swap every `EXAMPLE-*` ticker for
your real holdings. Fill in your actual NLV, position counts, and
sleeve assignments. Every `# TODO` field in `examples/state.schema.yaml`
must be populated.

### 3. Set your parameters
Your `state.yaml` defines:
- `sleeves.a.*` — ETF targets, within-sleeve weights, thematic cap
- `sleeves.b.ceiling_pct` — satellite ceiling (cap-only: no target, no Top-up)
- `sleeves.c.duration_band` — the one control inside the defensive sleeve
- `glidepath` — age bands: total risky weight, B ceiling, C targets
- `constraints` — look-through company cap, sector cap, tolerance, US-situs ceiling
- `monthly_dca` — your monthly contribution

### 4. Paste the init prompt + your snapshot
Open a fresh AI chat, paste `lios/v4.1.md` as the first message,
then paste your snapshot (generated or hand-crafted from your state).
The AI acknowledges LIOS v4.1 and loads your holdings. Then paste any
command from `commands/`.

### 5. Keep your personal data private
Your real `state.yaml`, snapshots, research, and reports belong in
a **private repo** (not this one). The public LIOS repo carries only
the framework, synthetic examples, and documentation — never contains
real portfolio data. See [ADR-003](./decisions/ADR-003.md) for the
full bifurcation design.

---

## Repository Structure

```
LIOS/                              ← public framework repo
├── README.md                      ← You are here
├── VERSION.md                     Current release: 5.0.0
├── CHANGELOG.md                   Version history
├── LICENSE                        Open-source (MIT)
├── constitution/                  Permanent knowledge — rarely changes
│   ├── constitution.md            The constitution: eight rules, P1′–P8′
│   ├── method.md                  Scoring, reports, workflow — editable without a bump
│   ├── mission.md                 Core purpose
│   ├── principles.md              Communication standards
│   └── governance.md              "Rules permanent, decisions dynamic"
├── core/                          Framework implementation
│   ├── sleeves.md                 Four sleeves A/B/C, constraints, funding matrix
│   ├── deployment.md              Top-up and Rebalance — the two operations
│   ├── regime-index.md            Regime Index (0–100, dashboard only)
│   ├── glidepath.md               Age bands, step-up, decumulation
│   ├── kelly.md                   RETIRED — v2.0 Kelly Model stub + rationale
│   ├── strategic-reserve.md       RETIRED — v2.x SR stub + rationale
│   ├── replacement-index.md       Sleeve B replacement rules
│   ├── research-layer.md          LOS/RIL scoring framework
│   ├── health-check.md            14-section structure
│   ├── investment-committee.md    IC authority + workflow
│   ├── execution-engine.md        PCI / LFS integration layer
│   ├── validation.md              11-check auto-validation
│   └── event-pipeline.md          LIOS workflow
├── examples/                      Synthetic demo data
│   ├── state.schema.yaml          Empty contract — every field with # TODO
│   ├── state.example.yaml         Fully synthetic populated demo
│   └── snapshots/                 Generated synthetic snapshot
├── lios/                          Paste-ready init prompts
│   ├── v4.1.md                    Current init (v4.1)
│   ├── v2.1.md                    Frozen history — not referenced by v4.1
│   ├── v2.0.md                    Frozen history — not referenced by v4.1
│   └── components/                Modular reference docs
├── commands/                      Paste-ready command library
├── scores/                        Quick-reference score methodology cards
├── playbooks/                     Agent workflow definitions
├── templates/                     Reusable document templates
├── rfcs/                          Proposed changes (RFC-001 … RFC-005)
├── decisions/                     Architecture Decision Records (ADR-001 … ADR-007)
└── docs/
    └── glossary.md
```

> **Note:** The original LIOS repo (now `kccleoc/LIOS-archive`, private) carries
> Leo's full v1.x–v2.1.2 history — including v1.x historical prompts, every portfolio
> snapshot, all research, and the complete git commit graph. The public framework
> repo starts at v2.0.0 and carries no personal portfolio data.

---

## How LIOS Allocates Capital

```
LOS       →  decides WHICH assets deserve investment
LFS       →  decides PRIORITY among investments that pass LOS
Glidepath →  sets TOTAL risky exposure by age (P2′)
Top-up    →  allocates this month's contribution to the gaps (never sells)
Rebalance →  corrects a band or cap breach (sells, direction-limited by P4′)
Sleeves   →  decide WHERE the capital lives (A/B/C)
```

Two operations only. There is no reserve, no trigger ladder, no discretionary
channel, and no score that sizes anything. The Regime Index reports; it does not
act.


- **LOS** (v1.4): Research Intelligence Layer — LVS/LGS/LQS/LMS → LOS 0-100
- **LFS** (v1.5): Portfolio Construction Intelligence — five-dimension portfolio fit score

---

## LIOS Playbook

### Play 1 — Initialize (start every new session here)
Open any AI chat and paste `lios/v4.1.md` as the **first message**.
The AI acknowledges LIOS v4.1. Without this, the AI has no framework,
no rules, and will hallucinate.

### Play 2 — Load Your Portfolio
Paste your current portfolio snapshot (generated from your own
`state.yaml` via the snapshot format shown in
`examples/snapshots/2026-01-01_portfolio.md`). The AI now has
full context: rules, portfolio, cash, caps, sleeves.

### Play 3 — Run a Command
With LIOS initialized and portfolio loaded, paste any command from
`commands/`. Example workflow:
```
Paste commands/regime-recalc.md  →  AI returns the Regime Index band
Paste commands/health-check.md   →  AI returns full 14-section report
Paste commands/los-lfs.md        →  AI returns LOS + LFS for a candidate
```

### Play 4 — Follow Up
Commands are starting points, not endpoints. Challenge the AI's output,
refine scores, compare candidates, deep-dive into engine maps.
Every follow-up builds on prior answers within the session.

### Play 5 — Synthesize
Before leaving a session, ask: "What should I do next?" → CIO Resolution.
"What changed since last session?" → diff against prior snapshot.

### Play 6 — Export & Close
Paste `commands/portfolio-print.md`, copy the AI's output into
your private snapshot archive for the next session.

### Recommended Cadence

| When | Plays | Commands |
|---|---|---|
| **Weekly (5 min)** | 1 → 2 → 3 | `drift-check` |
| **Monthly (30 min)** | 1 → 2 → 3 → 4 | `los-sleeve-b` → `health-check` |
| **Quarterly (1 hr)** | 1 → 2 → 3 → 4 → 5 | Full review: `sleeve-c-review` → `portfolio-balance` → `economic-engine-map` → `health-check` |
| **On new idea** | 1 → 2 → 3 → 4 | `los` (if LOS ≥ 70) → `lfs` → `los-lfs` |
| **Every session end** | 6 | `portfolio-print` → save to your private snapshot archive |

---

## Core Principles

These never change across versions. They are the constitution.

1. **Conclusions first, reasoning second.**
2. **Exact dollar amounts always** — never percentages alone.
3. **Explicit directives:** Buy / Hold / Reduce / Exit.
4. **Facts separated from Assessments** in every report.
5. **Every recommendation carries a Confidence % with rationale.**
6. **No generic investing advice. No motivational language. No fluff.**
7. **Rules are permanent. Decisions are dynamic.**

---

## Evolving LIOS

1. **Propose** → file an RFC in `rfcs/`
2. **Discuss** → IC reviews the proposal
3. **Decide** → IC accepts, rejects, or defers
4. **Implement** → if accepted, execute the changes
5. **Record** → file an ADR in `decisions/` for architectural decisions
6. **Release** → bump VERSION.md, update CHANGELOG.md

- **Minor bumps** (v2.0 → v2.1): new capabilities, refined explanations. No constitutional changes.
- **Major bumps** (v2.x → v3.0): Locked Decision changes. Requires ADR + IC vote.
- **Patch bumps** (v2.0.0 → v2.0.1): bug fixes, typo corrections.

---

## Versioning

- **v6.0** (current) — Three sleeves: conviction ETFs in A, cap-only satellites in B, and one defensive sleeve C holding cash, bills and bond ETFs. C is the residual (100% − risky) with a duration band of 1–6 years and no holding above 10. Look-through caps. 14-section Health Check, 11 validation checks. Soft limits rebalance quarterly within a tolerance, escalating to hard at 3× tolerance or after two reviews. Constitution P1′–P8′.
- **v5.0** — Four sleeves: D deleted, duration in C, cash in Z. Superseded in v6 when C and Z merged.
- **v3.0** — Open-source bifurcation: public framework repo + private personal-data repo. L2/L3 reframed (form preserved, parameters moved to `state.yaml`). L7 removed (was portfolio-local). All framework files ticker-neutral. Synthetic `examples/` for cloner onboarding.
- **v2.1.2** — Computed-NLV SSOT; `portfolio/state/portfolio.yaml` canonical
- **v2.0.0** — Modular agent-native architecture
- **v1.5**   — Portfolio Construction Intelligence (LFS)
- **v1.4**   — Research Intelligence Layer (LOS)
- **v1.3**   — Locked Decisions (L1–L10) + full Health Check structure

See [`CHANGELOG.md`](./CHANGELOG.md) for the full history.

---

## Owner

Created by Lc / Leo — self-directed investor. LIOS v5.0 is open-source (MIT); use the framework for your own portfolio, adapt it, contribute back. This repo contains no financial advice and no real portfolio data.
