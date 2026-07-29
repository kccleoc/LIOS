# Domain Docs

LIOS stores domain knowledge in two places:

- `docs/glossary.md` — domain glossary and shared vocabulary
- `decisions/ADR-NNN.md` — Architecture Decision Records (ADR-001, ADR-002, …)

## Consumer Rules

When your output names a domain concept (in an issue title, a refactor proposal,
a hypothesis, a test name), use the term as defined in `docs/glossary.md`.
Don't drift to synonyms the glossary explicitly avoids.

If the concept you need isn't in the glossary yet, that's a signal — either
you're inventing language the project doesn't use (reconsider) or there's a
real gap (note it for inclusion via a PR).

## Flag ADR Conflicts

If your output contradicts an existing ADR, surface it explicitly rather than
silently overriding:

> _Contradicts ADR-002 (X) — but worth reopening because…_