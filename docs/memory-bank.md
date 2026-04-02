# Memory Bank

The Memory Bank is the **authoritative, project-local context** for this repository. It is not a scratchpad: it should stay accurate, current, and small enough to read quickly.

## Purpose

- Capture **decisions, constraints, and facts** that are easy to forget between sessions.
- Give humans and coding agents a **single place** to learn how this repo differs from upstream or generic assumptions.
- Reduce repeated exploration of the same files and the same “why did we do it this way?” questions.

## When to update (required)

**After each meaningful project change**, update the Memory Bank before considering the work finished. “Meaningful” includes:

- New or changed features, APIs, or user-visible behavior.
- Refactors that move or rename important modules.
- New dependencies, build steps, or test commands.
- Architectural or workflow decisions (including reversals of earlier decisions).
- Fixes to non-obvious bugs (what was wrong, what fixed it, what to avoid next time).

If you only fixed a typo in an unrelated comment, you do not need to log that. When in doubt, **prefer a short update** over leaving stale documentation.

## What “updated” means

- **Replace or delete** statements that are no longer true; do not append contradictory notes without reconciling them.
- Prefer **concrete paths, commands, and names** over vague descriptions.
- Keep each document **focused**: move long digressions into a supplementary file or trim them.

## Optional supplementary files

This file (`docs/memory-bank.md`) holds the pattern and high-signal notes. For larger topics, add Markdown files under `docs/` (e.g. `docs/memory-bank/` or `docs/project-brief.md`). Names are conventional; adjust if the team prefers fewer files.

| File (example) | Contents |
| --- | --- |
| `project-brief.md` | Goals, scope, non-goals, and success criteria for this fork or effort. |
| `product-context.md` | Who uses the project, main workflows, and UX constraints. |
| `system-patterns.md` | Architecture choices, invariants, and patterns (e.g. how state flows, module boundaries). |
| `tech-context.md` | Stack, tooling, how to build/test, environment notes, and repo-specific commands. |
| `active-context.md` | What you are working on _now_, open questions, and the next concrete steps. |
| `progress.md` | What shipped recently, known issues, and follow-ups. |

You may start with a subset and grow. **Do not** duplicate `AGENTS.md` verbatim; link to it from `tech-context.md` or `project-brief.md` when helpful, and put fork-specific detail here or in supplementary files.

## How agents should use this

1. **Read** `docs/memory-bank.md` and any supplementary files at the start of non-trivial work in this repo.
2. **Write** updates as part of the same change that alters behavior or structure.
3. If something in the Memory Bank conflicts with the code, **treat the code as source of truth** and fix the Memory Bank (or escalate if the code is wrong).

Stale Memory Bank content is worse than none—keep it current.

## Workspace notes

- **`.vscode/settings.json`**: `files.associations` maps `*.mdc` (e.g. Cursor rules under `.cursor/rules/`) to the `markdown` language for syntax highlighting. `editor.tokenColorCustomizations.textMateRules` marks Markdown headings (`markup.heading.markdown`) as **bold** so they stay visible if the theme barely differentiates heading color.
