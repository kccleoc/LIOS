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

1. A **capital architecture** — five distinct layers (A/B/C/D/SR) with explicit funding rules and caps
2. A **scoring framework** — Kelly Model v2.0 (5 dimensions) + Replacement Index (3 metrics)
3. A **report structure** — 16-section Health Check, fixed order, always
4. A **governance philosophy** — "Rules are permanent. Decisions are dynamic."

The system is executed in parallel across any LLM. Every session reads the same initialization prompt → identical framework, identical voice, identical evaluation discipline.

---

## Quick Start

| What you want to do | Go to |
|---|---|
| Initialize a new AI session | [`lios/v2.1.md`](./lios/v2.1.md) — paste the entire file (v2.0.md also available for compat) |
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

### 3. Set your Locked Decision parameters
Your `state.yaml` defines:
- `sleeve_a.allocation` — your Sleeve A ETFs + within-Sleeve weights (L2 form)
- `sleeve_b_caps` — your Sleeve B tickers + binding NLV caps (L3 form)
- `sleeve_a.permanently_exited` — your permanently-exited list (was L7; now portfolio-local)
- `sleeve_c.target_pct_of_abc` / `sleeve_d.target_pct_of_total_nlv` — your target percentages
- `monthly_dca` — your monthly dollar-cost-average amount

### 4. Paste the init prompt + your snapshot
Open a fresh AI chat, paste `lios/v2.1.md` as the first message,
then paste your snapshot (generated or hand-crafted from your state).
The AI acknowledges LIOSv2.1 and loads your holdings. Then paste any
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
LIOS/                              ← public framework repo (v3.0)
├── README.md                      ← You are here
├── VERSION.md                     Current version: 3.0
├── CHANGELOG.md                   Version history
├── LICENSE                        Open-source (MIT)
├── constitution/                  Permanent knowledge — rarely changes
│   ├── constitution.md            All Locked Decisions (L1–L18; L7 gapped — removed v3.0)
│   ├── mission.md                 Core purpose
│   ├── principles.md              Communication standards
│   └── governance.md              "Rules permanent, decisions dynamic"
├── core/                          Framework implementation
│   ├── kelly.md                   Kelly Model v2.0
│   ├── sleeves.md                 Sleeves A/B/C/D architecture
│   ├── strategic-reserve.md       SR funding + deployment rules
│   ├── replacement-index.md       Sleeve D surgery rules
│   ├── research-layer.md          LOS/RIL scoring framework
│   ├── health-check.md            16-section structure
│   ├── investment-committee.md    IC authority + workflow
│   ├── execution-engine.md        PCI / LFS integration layer
│   ├── validation.md              9-check auto-validation
│   └── event-pipeline.md          15-step LIOS workflow
├── examples/                      Synthetic demo data (v3.0 — new)
│   ├── state.schema.yaml          Empty contract — every field with # TODO
│   ├── state.example.yaml         Fully synthetic populated demo ($1M NLV, EXAMPLE-* tickers)
│   └── snapshots/                 Generated synthetic snapshot
├── lios/                          Paste-ready init prompts
│   ├── v2.1.md                    Current init (v2.1)
│   ├── v2.0.md                    Previous (still valid for compat)
│   └── components/                Modular reference docs
├── commands/                      Paste-ready command library
├── scores/                        Quick-reference score methodology cards
├── playbooks/                     Agent workflow definitions
├── templates/                     Reusable document templates
├── rfcs/                          Proposed changes (RFC-001, RFC-002)
├── decisions/                     Architecture Decision Records (ADR-001, ADR-002, ADR-003)
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
LOS      →  decides WHICH assets deserve investment
LFS      →  decides PRIORITY among investments that pass LOS
Kelly    →  decides HOW MUCH capital to deploy
Sleeves  →  decide WHERE the capital lives (A/B/C/D/SR)
SR       →  offensive dry powder, deployed on triggers only
```

- **LOS** (v1.4): Research Intelligence Layer — LVS/LGS/LQS/LMS → LOS 0-100
- **LFS** (v1.5): Portfolio Construction Intelligence — five-dimension portfolio fit score

---

## LIOS Playbook

### Play 1 — Initialize (start every new session here)
Open any AI chat and paste `lios/v2.1.md` as the **first message**.
The AI acknowledges LIOS v2.1. Without this, the AI has no framework,
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
Paste commands/kelly-recalc.md  →  AI returns Kelly Factor
Paste commands/health-check.md  →  AI returns full 16-section report
Paste commands/los-lfs.md       →  AI returns LOS + LFS for a candidate
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
| **Monthly (30 min)** | 1 → 2 → 3 → 4 | `kelly-recalc` → `health-check` → `los-sleeve-b` |
| **Quarterly (1 hr)** | 1 → 2 → 3 → 4 → 5 | Full review: `sleeve-d-review` → `portfolio-fit` → `portfolio-balance` → `economic-engine-map` → `health-check` |
| **On new idea** | 1 → 2 → 3 → 4 | `los` (if LOS ≥ 70) → `lfs` → `los-lfs` → `kelly-recalc` (if LOS ≥ 80) |
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

- **v3.0** (current) — Open-source bifurcation: public framework repo + private personal-data repo. L2/L3 reframed (form preserved, parameters moved to `state.yaml`). L7 removed (was portfolio-local). All framework files ticker-neutral. Synthetic `examples/` for cloner onboarding.
- **v2.1.2** — Computed-NLV SSOT; `portfolio/state/portfolio.yaml` canonical
- **v2.0.0** — Modular agent-native architecture
- **v1.5**   — Portfolio Construction Intelligence (LFS)
- **v1.4**   — Research Intelligence Layer (LOS)
- **v1.3**   — Locked Decisions (L1–L10) + full Health Check structure

See [`CHANGELOG.md`](./CHANGELOG.md) for the full history.

---

## Owner

Created by Lc / Leo — self-directed investor. LIOS v3.0 is open-source (MIT); use the framework for your own portfolio, adapt it, contribute back. This repo contains no financial advice and no real portfolio data.
