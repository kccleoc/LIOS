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

**LIOS v7.0.1 — adopted 2026-11-18. Supersedes v6.0.5 and every earlier version.**
**Status:** in force from adoption.

---

## What is LIOS?

LIOS is **not a stock-picking service**. It is:

1. A **capital architecture** — three sleeves (A/B/C) measured against one denominator, with a constraint set that outranks every rule
2. A **research discipline** — written theses and view statements, thesis-break exits established by documented research and verification; there is no score
3. A **deployment engine** — exactly two operations: Top-up and Rebalance
4. A **report structure** — 14-section Health Check, fixed order, always
5. A **constitution** — eight rules, P1′–P8′, plus subordinate P3a, and nothing else

The system is executed in parallel across any LLM. Every session reads the same initialization prompt → identical framework, identical voice, identical evaluation discipline.

---

## Quick Start

| What you want to do | Go to |
|---|---|
| Initialize a new AI session | [`constitution/constitution.md`](./constitution/constitution.md) — Part I, paste it first |
| Set up your own portfolio | Copy [`examples/state.example.yaml`](./examples/state.example.yaml) → rename → swap tickers → save as your `state.yaml` |
| Understand the state contract | [`examples/state.schema.yaml`](./examples/state.schema.yaml) — every field with `# TODO` descriptions |
| See a demo snapshot | [`examples/snapshots/2026-01-01_portfolio.md`](./examples/snapshots/2026-01-01_portfolio.md) — synthetic example |
| Run a quick command | [`commands/README.md`](./commands/README.md) |
| Understand one concept deeply | [`docs/glossary.md`](./docs/glossary.md) |
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
- `glidepath` — age bands: total risky weight, B ceiling
- `constraints` — look-through company cap, sector cap, tolerance, US-situs ceiling
- `monthly_dca` — your monthly contribution

### 4. Paste the init prompt + your snapshot
Open a fresh AI chat, paste `constitution/constitution.md` (Part I) as the
first message, then paste your snapshot (generated or hand-crafted from your
state). The AI acknowledges LIOS v7 and loads your holdings. Then paste any
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
├── VERSION.md                     Current release: 7.0.1
├── CHANGELOG.md                   Version history
├── LICENSE                        Open-source (MIT)
├── constitution/                  Permanent knowledge — rarely changes
│   ├── constitution.md            The constitution: eight rules, P1′–P8′, plus P3a
│   ├── method.md                  Quarterly review, thesis breaks, reporting
│   ├── mission.md                 Core purpose
│   └── principles.md              Communication standards
├── core/                          Framework implementation
│   ├── sleeves.md                 Three sleeves A/B/C, constraints
│   ├── deployment.md              Top-up and Rebalance — the two operations
│   ├── glidepath.md               Age bands, step-up, decumulation
│   ├── health-check.md            14-section structure
│   ├── investment-committee.md    IC authority + workflow
│   ├── agent-role.md              Part VII — agent role and guardrails
│   └── validation.md              30-check auto-validation
├── examples/                      Synthetic demo data
│   ├── state.schema.yaml          Empty contract — every field with # TODO
│   ├── state.example.yaml         Fully synthetic populated demo
│   └── snapshots/                 Generated synthetic snapshot
├── commands/                      Paste-ready command library (V7 only)
├── templates/                     Reusable document templates
├── rfcs/                          Proposed changes (archive)
├── decisions/                     Architecture Decision Records (ADR-001 … ADR-009)
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
Theses     →  decide WHICH assets deserve investment (written, verified)
Glidepath  →  sets TOTAL risky exposure by age (P2′)
Top-up     →  allocates this month's contribution to the gaps (never sells)
Rebalance  →  corrects a band or cap breach (sells, direction-limited by P4′)
Sleeves    →  decide WHERE the capital lives (A/B/C)
```

Two operations only. There is no reserve, no discretionary
channel, and no score that sizes anything. The Regime Index reports; it does not
act. There is no score in V7.

---

## LIOS Playbook

### Play 1 — Initialize (start every new session here)
Open any AI chat and paste `constitution/constitution.md` (Part I) as the
**first message**. The AI acknowledges LIOS v7. Without this, the AI has no
framework, no rules, and will hallucinate.

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
Paste commands/drift-check.md    →  AI returns the gap table
```

### Play 4 — Follow Up
Commands are starting points, not endpoints. Challenge the AI's output,
refine theses, compare candidates, deep-dive into the evidence.
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
| **Monthly (30 min)** | 1 → 2 → 3 → 4 | `health-check` |
| **Quarterly (1 hr)** | 1 → 2 → 3 → 4 → 5 | Full review: `sleeve-c-review` → `health-check` |
| **On new idea** | 1 → 2 → 3 → 4 | Research the thesis → IC decision |
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
4. **Implement** → if accepted, execute the changes (Muse implements directly after owner approval)
5. **Record** → file an ADR in `decisions/` for architectural decisions
6. **Release** → bump VERSION.md, update CHANGELOG.md

What counts as a major, minor, or patch bump is defined by the single amendment table in Part I — this file does not restate it.

See the single amendment table in Part I (`constitution/constitution.md`). It is the only amendment table.

---

## Versioning

- **v7.0** (current) — Three sleeves: ETFs in A, cap-only satellites in B, and one defensive sleeve C holding cash, bills and bond ETFs. C is the residual with a duration band of 1–6 years and no holding above 10y. Look-through caps. 14-section Health Check, 30 validation checks. No score: B exits only on thesis break, established by documented research and verification. Mandate windows bounded by 31 January, 31 July, 31 October. Constitution P1′–P8′ + P3a.
- **v6.0** — Three sleeves: conviction ETFs in A, cap-only satellites in B, and one defensive sleeve C holding cash, bills and bond ETFs. C is the residual with a duration band of 1–6 years and no holding above 10. Look-through caps. 14-section Health Check, 12 validation checks. Constitution P1′–P8′.
- **v5.0** — D deleted, duration in C. Superseded in v6 when C and the cash sleeve merged.
- **v3.0** — Open-source bifurcation: public framework repo + private personal-data repo. All framework files ticker-neutral. Synthetic `examples/` for cloner onboarding.
- **v2.1.2** — Computed-NLV SSOT; `portfolio/state/portfolio.yaml` canonical
- **v2.0.0** — Modular agent-native architecture
- **v1.5**   — Portfolio Construction Intelligence
- **v1.4**   — Research Intelligence Layer
- **v1.3**   — Locked Decisions + full Health Check structure

See [`CHANGELOG.md`](./CHANGELOG.md) for the full history.

---

## Owner

Created by Lc / Leo — self-directed investor. LIOS v7.0 is open-source (MIT); use the framework for your own portfolio, adapt it, contribute back. This repo contains no financial advice and no real portfolio data.
