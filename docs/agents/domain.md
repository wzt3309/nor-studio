# Domain docs

## Layout

This repository uses a single-context layout:

- `CONTEXT.md`: domain vocabulary and concepts.
- `docs/adr/`: architectural decision records.

## Reading rules

Before exploring a domain area, read `CONTEXT.md` and relevant ADRs.

If these files are absent, proceed silently. Create them when domain terms or
architectural decisions are resolved, using the domain-modeling skill.

## Vocabulary and decisions

Use the vocabulary defined in `CONTEXT.md` when naming domain concepts in issues,
proposals, code, and tests. Surface meaningful vocabulary gaps for domain modeling.

If a proposal conflicts with an ADR, identify the conflict and explain why the
decision should be reconsidered.
