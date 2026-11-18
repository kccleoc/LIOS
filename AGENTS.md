# AGENTS.md — LIOS Framework Quick Reference (v7.0.0)

**LIOS v7.0.0 — adopted 2026-11-18. Supersedes v6.0.5 and every earlier version.**
**Status:** in force from adoption.

This is a **documentation-only repo**. No code, no build, no tests,
no linter, no CI. The framework is a set of version-controlled rules
and paste-ready prompts for running the owner's portfolio.

If you're an agent expecting `package.json`, `go test`, `pytest`,
or build artifacts — there are none. Adjust expectations.

---

## Who implements LIOS now

The repo AI agent **no longer handles LIOS**. The owner revoked that
arrangement. V7 is implemented directly by Muse as the owner's advisor,
after the owner's approval — commit, push, tag — in the private repo.
Any instruction in this repo that assumes the repo agent amends the
framework is superseded. Do not follow the old re-pin or push-framework
flow as an agent instruction; `bin/push-framework.sh` is a script the
owner runs, not a standing delegation.

---

## Critical rules

- **No commit without explicit owner request, per request.** Approval is
  per request, never standing. Stage freely, but never run `git commit`
  or `git push` unless explicitly asked for that specific change.
- **The agent never trades.** It never places, modifies, or cancels an
  order. Execution is the owner's act alone (P3′). This is a
  constitutional rule, not a convention.
- **Eight Portfolio Rules, P1′–P8′, plus subordinate P3a, are constitutional**
  (see `constitution/constitution.md`, Part I). That file is the sole
  constitution. In any conflict between it and any other file, it governs.
  - **The eight rules:** P1′ three sleeves, one denominator, and the scope
    of the book · P2′ targets from the glidepath, risky weight by age,
    Sleeve B cap-only for life · P3′ two operations only (Top-up,
    Rebalance), soft limits quarterly within tolerance, hard caps fire
    immediately · P3a transition mandate (windowed, eight requirements) ·
    P4′ Rebalance direction limits (A → C prohibited) · P5′ exits follow
    the instrument: B exits only on a thesis break, established by
    documented research and verification; there is no score · P6′ caps
    outrank every rule · P7′ a cap breach is a Rebalance trigger with a
    10-business-day cure-or-escalate deadline and an expiry on every
    accepted breach · P8′ reporting: 14 sections, dollars, confidence.
- **One amendment table, in Part I.** It is the only amendment table.
  This file does not restate it, and no other file contains one. If a
  change is arguably a rule, it is a rule: major version bump + ADR +
  CHANGELOG entry. A P-rule amendment takes effect 30 days after its ADR
  is recorded. Portfolio-local facts in the SSOT (positions, shares, cost
  bases, market values, notes, external-account listings) need no bump;
  they take a dated record in the SSOT history.
- **Method text that changes no rule, no date, no threshold, and no
  deadline** needs a CHANGELOG entry only. The quarterly review calendar,
  marks convention, and agenda order are pinned: changing them is a
  constitutional amendment (major bump + ADR).
- **State the framework version before acting.** Confirm
  `framework/VERSION` is v7.0.0 before reading the framework or running
  a validation. An agent that has not confirmed the version has not read
  the framework.
- **Make every output reproducible by a second session.** Record the
  inputs: fund holding weights and their dates for every look-through
  computation, cap-computation inputs, thesis-break evidence with sources
  and dates, and the prices and dates behind every re-mark. An output
  whose inputs are not recorded cannot be checked, and an output that
  cannot be checked is not a control.
- **Three sleeves, one denominator.** A, B, C measured against NLV.
  There is no fourth sleeve. B is cap-only and receives no Top-up, ever.
  C is the residual. The book is the IBKR portfolio; Mox is operating
  cash outside the book, and Mox securities are listed in the SSOT with
  their US-situs total so every cap is tested on the true figure.
- **Two file roles, two rules:**
  - **`commands/*.md`** = paste-ready prompts. They get pasted into fresh
    AI sessions with no other context. **Must be self-contained.** Don't
    link to other files from inside code blocks — readers won't see the
    link targets.
  - **Reference docs** (`README.md`, `CHANGELOG.md`, `docs/glossary.md`,
    `decisions/ADR-NNN.md`) = human reference. Relative links are fine
    and encouraged.
- **v3.0 cross-repo model.** The public repo (`kccleoc/LIOS`) contains
  the framework only — no personal portfolio data. Personal state,
  snapshots, research, reports, and the vendored framework copy live
  in the private data repo (`kccleoc/LIOS-mine`). Never push personal
  portfolio data to the public repo.

---

## File conventions

| Type | Pattern | Example |
|---|---|---|
| Constitutional | `constitution/*.md` — rarely changed, requires major bump | `constitution/constitution.md` |
| Core framework | `core/*.md` — rarely changed, requires major bump | `core/deployment.md` |
| Agent role | `core/agent-role.md` — Part VII | `core/agent-role.md` |
| Commands | `commands/*.md` — paste-ready prompts (V7 only) | `commands/health-check.md` |
| Templates | `templates/*.md` — structural, rarely changed | `templates/ADR.md` |
| Examples | `examples/*.yaml`, `examples/snapshots/*.md` — synthetic demo data | `examples/state.example.yaml` |
| RFCs | `rfcs/RFC-NNN.md` — proposed changes (archive) | `rfcs/RFC-001.md` |
| ADRs | `decisions/ADR-NNN.md` — permanent architectural decisions | `decisions/ADR-009.md` |
| Docs | `docs/*.md` — glossary and agent reference | `docs/glossary.md` |

**Archives — do not edit, do not follow as live rules:** `lios/v2.0.md`,
`lios/v2.1.md`, `lios/v4.1.md` and `lios/components/*.md` are frozen
historical version documents. `rfcs/RFC-NNN.md` and
`decisions/ADR-001.md` through `decisions/ADR-008.md` are the historical
record. A document may describe an old version only when it is describing
the past.

---

## Version bumps

See the single amendment table in Part I
(`constitution/constitution.md`). It covers every file and every kind of
change. Do not restate it here, and do not invent a second table
anywhere else.

---

## Verification after edits

Run before claiming a multi-file change is done:

```bash
# 1. Cross-link sanity (catches broken `../foo.md` refs)
grep -rn "components/\|commands/\|snapshots/\|docs/" --include="*.md" .

# 2. No stale version refs (live files point at the current version;
#    the "Supersedes v6.0.5" header line in the constitution and method is
#    the adoption record, not a stale ref)
grep -rn "v6\.0\.5\|v5\.0\.0" --include="*.md" framework/ | grep -v "CHANGELOG.md\|decisions/ADR-00\|lios/\|rfcs/" | grep -v "Supersedes v6"

# 3. No superseded mechanism named as a live rule in live framework files
grep -rniE "Satellite Scoreboa""rd|in either directio""n|one-time structural migratio""n" --include="*.md" framework/constitution framework/core framework/commands   # (strings split so this check does not itself contain them)

# 4. Leak prevention (must pass before any public push)
bin/push-framework.sh <tag> --dry-run   # runs the leak gate without pushing
```

The leak gate (`bin/push-framework.sh`, private repo) generates its
ticker list from the SSOT's current holdings plus the permanently-exited
list, and its dollar check from the SSOT's current dollar figures. Never
write real tickers or live portfolio figures into a framework file —
Part I contains no live portfolio data at all, and the gate must pass on
the whole framework tree before pushing.

---

## Voice / tone for new content

Match the existing institutional, terse voice:

- Conclusions first, reasoning second
- Dollar amounts always — never percentages alone
- Explicit directives: Buy / Hold / Reduce / Exit / Pass / Watch
- Facts separated from Assessments
- Confidence % on every recommendation
- No fluff, no motivational language, no emoji

Full standard: `constitution/principles.md`.
When in doubt, read 2-3 existing files in the same directory
before writing a new one.

---

## When adding a new LIOS version (workflow)

1. Draft the new version as the owner's advisor; the owner reads it in
   detail before it goes anywhere
2. Owner approves the draft in full — only then implement
3. Major version bump + ADR in `decisions/` + CHANGELOG entry
   (see the amendment table in Part I — it is the only table)
4. Update `framework/VERSION` and `framework/VERSION.md`
5. Update `docs/glossary.md` with any new terms; remove terms deleted
   by the new version
6. Run all four verification greps above, plus the leak gate dry-run
7. Commit and push only with the owner's per-request approval; tag only
   after a fresh hostile review of the implemented files passes
8. A hostile review's findings are tracked as queue items in
   `execution/pending.yaml` until closed — a review whose findings are
   not tracked is a review that did not happen

---

## Agent skills

### Issue tracker

Issues live in GitHub Issues (kccleoc/LIOS) via the `gh` CLI. See `docs/agents/issue-tracker.md`.

### Triage labels

Default vocabulary: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`. See `docs/agents/triage-labels.md`.

### Domain docs

Vocabulary: `docs/glossary.md`. ADRs: `decisions/ADR-NNN.md`. See `docs/agents/domain.md`.
