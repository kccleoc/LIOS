# AGENTS.md — LIOS Repo Quick Reference

This is a **documentation-only repo**. No code, no build, no tests,
no linter, no CI. The "output" of this repo is paste-ready AI prompts
that get copied into fresh chat sessions (Claude, ChatGPT, etc.) to
run Lc's portfolio management framework.

If you're an agent expecting `package.json`, `go test`, `pytest`,
or build artifacts — there are none. Adjust expectations.

---

## Critical rules

- **No commit without explicit user request.** Lc has a strict
  no-auto-commit rule. Stage freely (`git add`), but never run
  `git commit` or `git push` unless explicitly asked.
- **Eight Portfolio Rules, P1–P8, are constitutional** (see
  `constitution/constitution.md`). Do not propose changes that violate them
  without flagging a major version bump plus an ADR.
  - **The eight rules:** P1 five sleeves one denominator · P2 targets from the
    glidepath, risky weight by age · P3 two operations only (Top-up, Rebalance) ·
    P4 Rebalance direction limits · P5 Sleeve B exits only on thesis break, via a
    30-day review · P6 caps outrank every rule · P7 a cap breach is a Rebalance
    trigger · P8 reporting.
  - **Method lives elsewhere.** `constitution/method.md` holds LOS/LFS weights,
    bands, the Replacement Index and report templates. Change it with a CHANGELOG
    entry — no version bump.
  - **Parameters** in `state.yaml` (tickers, weights, caps, DCA, glidepath bands)
    need no bump. **Rules** in `constitution/` and `core/` require a major bump.
  - There are no L-numbers any more. If you see one in an old file, treat it as
    historical.
- **Two file roles, two rules:**
  - **`lios/v*.md` and `commands/*.md`** = paste-ready prompts.
    They get pasted into fresh AI sessions with no other context.
    **Must be self-contained.** Don't link to other files from
    inside code blocks — readers won't see the link targets.
  - **`lios/components/*.md`, `README.md`, `CHANGELOG.md`,
    `docs/glossary.md`** = human reference docs.
    Relative links (`../commands/foo.md`) are fine and encouraged.
- **v3.0 cross-repo model.** This public repo (`kccleoc/LIOS`) contains
  the framework only — no personal portfolio data. Personal state,
  snapshots, research, reports, and the vendored framework copy live
  in a private data repo (`kccleoc/LIOS-mine` for Leo). The original
  v1.x–v2.1.2 history lives frozen in `kccleoc/LIOS-archive` (private,
  renamed). Never push personal portfolio data to this public repo.

---

## File conventions

| Type | Pattern | Example |
|---|---|---|
| Constitutional | `constitution/*.md` — rarely changed, requires major bump | `constitution/constitution.md` |
| Core framework | `core/*.md` — rarely changed, requires major bump | `core/deployment.md` |
| Init prompts | `lios/vX.Y.md` — paste-ready initialization files | `lios/v3.0.md` |
| Components | `lios/components/*.md` — modular reference docs | `lios/components/02_capital_architecture.md` |
| Commands | `commands/*.md` — paste-ready prompts | `commands/health-check.md` |
| Scores | `scores/*.md` — quick reference score methodology | `scores/LOS.md` |
| Playbooks | `playbooks/*.md` — agent workflow definitions | `playbooks/HealthCheck.md` |
| Templates | `templates/*.md` — structural, rarely changed | `templates/ADR.md` |
| Examples | `examples/*.yaml`, `examples/snapshots/*.md` — synthetic demo data (v3.0) | `examples/state.example.yaml` |
| RFCs | `rfcs/RFC-NNN.md` — proposed changes | `rfcs/RFC-001.md` |
| ADRs | `decisions/ADR-NNN.md` — permanent architectural decisions | `decisions/ADR-001.md` |
| Docs | `docs/*.md` — glossary and agent reference | `docs/glossary.md` |


**Never rename or delete `lios/v1.X.md` or `archive/v1.X/` files** — they are the
historical record (preserved in `kccleoc/LIOS-archive`, frozen). This public repo
carries `lios/v3.0.md` as current, with `v2.0.md` and `v2.1.md` frozen and
unreferenced. CHANGELOG.md marks supersession.

---

## Version bumps

| Bump type | When | Required actions |
|---|---|---|---|
| **Minor** | New module, new commands, refined docs, snapshot updates | 1. RFC in `rfcs/`  2. CHANGELOG entry  3. Update affected files  4. Update `constitution/constitution.md` if a new L-number is added |
| **Major** | A P-rule change (P1–P8) | Same as minor + ADR in `decisions/` + constitutional amendment note in CHANGELOG |
| **Patch** | Typo fixes, small corrections | CHANGELOG entry only |

---

## Verification after edits

Run before claiming a multi-file change is done:

```bash
# 1. Cross-link sanity (catches broken `../foo.md` refs)
grep -rn "components/\|commands/\|snapshots/\|docs/" --include="*.md" .

# 2. No stale version refs (paste-ready files should point at current version)
grep -rn "v1.3.md" --include="*.md" . | grep -v "superseded\|previous"

# 3. Leak prevention (v3.0 — must return zero on public repo; CI-enforceable)
grep -rE 'NVDA|PLTR|CRWD|GEV|BOTZ|RMBS|QTUM|\bMU\b|GOOG|IBTA|VWRA|CSPX|IB01|COHR|MRVL' . \
    | grep -vE 'constitution/constitution.md.*pre-v3|docs/glossary.md.*pre-v3'
grep -rE '\$[0-9]{4,}' . \
    | grep -v 'examples/snapshots'
```

---

## Voice / tone for new content

Match the existing institutional, terse voice:

- Conclusions first, reasoning second
- Dollar amounts always — never percentages alone
- Explicit directives: Buy / Hold / Reduce / Exit / Pass / Watch
- Facts separated from Assessments
- Confidence % on every recommendation
- No fluff, no motivational language, no emoji

Full standard: `lios/components/01_identity_and_communication.md`.
When in doubt, read 2-3 existing files in the same directory
before writing a new one.

---

## When adding a new LIOS version (workflow)

1. `cp lios/v1.X.md lios/v1.X+1.md`
2. Edit heading: bump version + add changelog banner at top
3. Insert new module content before the Portfolio Rules section
4. Add new Portfolio Rules (P9, P10…) if required — a major bump with an ADR
5. Update CHANGELOG.md with a new top entry (most recent first)
6. Update affected `lios/components/*.md` (add new ones, modify
   referenced sections in existing ones)
7. Update README.md structure tree
8. Update `docs/glossary.md` with any new terms
9. Run all three verification greps above
10. Stage but **don't commit** — wait for user OK

---

## Agent skills

### Issue tracker

Issues live in GitHub Issues (kccleoc/LIOS) via the `gh` CLI. See `docs/agents/issue-tracker.md`.

### Triage labels

Default vocabulary: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`. See `docs/agents/triage-labels.md`.

### Domain docs

Vocabulary: `docs/glossary.md`. ADRs: `decisions/ADR-NNN.md`. See `docs/agents/domain.md`.