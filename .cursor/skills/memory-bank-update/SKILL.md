---
name: memory-bank-update
description: Updates the Memory Bank with recent changes so technical details match the repo. Use when the user asks to update the memory bank, sync documentation, refresh project context, or align docs after features or refactors.
---

# Memory Bank update

## When to use

After significant code changes: new features, refactors, architecture shifts, dependency or tooling updates, or completed milestones. Triggers include: "update memory bank", "sync docs", "refresh project docs".

## Inputs

- What changed: from `git diff`, recent commits, conversation, or the user’s description.

## Canonical layout (this repo)

Read **`docs/memory-bank.md`** first. It defines the pattern and optional supplementary Markdown under **`docs/`** or **`docs/memory-bank/`** (kebab-case names).

| Supplementary file (examples) | Use when                                    |
| ----------------------------- | ------------------------------------------- |
| `project-brief.md`            | Scope, goals, non-goals                     |
| `product-context.md`          | Users, workflows, UX constraints            |
| `system-patterns.md`          | Architecture, invariants, module boundaries |
| `tech-context.md`             | Stack, build/test commands, tooling         |
| `active-context.md`           | Current focus, open questions, next steps   |
| `progress.md`                 | What shipped, known issues, follow-ups      |

If a file does not exist yet, create it only when the change warrants a dedicated doc; otherwise update `docs/memory-bank.md` only.

## Steps

1. **Recent changes**: Run `git log -10 --oneline` and `git diff --stat HEAD~5..HEAD` (or `git diff --stat` against the branch base if `HEAD~5` is unavailable). Use this plus any user/conversation context to list touched areas.
2. **Read** `docs/memory-bank.md` and any existing supplementary files under `docs/` and `docs/memory-bank/`.
3. **Map updates** (create or edit the matching file(s)):
   - New feature / milestone → `progress.md` + `active-context.md` (or `docs/memory-bank.md` if those files are absent).
   - Architecture / invariants → `system-patterns.md`; record notable reversals or tradeoffs in the same file or a short `decision-log.md` under `docs/` **only if** the repo already uses one.
   - Dependencies / build / CI → `tech-context.md` (and `docs/memory-bank.md` if commands belong in the main index).
   - Scope / product direction → `project-brief.md` + `product-context.md`.
4. **Verify** every technical claim against source (files, `package.json`, scripts). If docs and code disagree, **fix the docs** unless the code is wrong.
5. **Length**: keep each file **under 200 lines**; summarize, link paths, or split into another supplementary file instead of bloating one doc.

## Outputs

- List of updated Memory Bank files (paths).
- Short summary of what changed in the docs and why.

## Safety

- Do **not** remove manually curated content without confirming with the user.
- Do **not** add guesses or speculation—only verified facts.
- Do **not** exceed ~200 lines per file without splitting or trimming.
- **Code is source of truth** until intentionally changed; documentation follows.

## Agent behavior

- **Execute** git commands from the repository root when gathering diffs.
- After updating, ensure statements in `docs/memory-bank.md` still describe the overall pattern if supplementary files were added or renamed.
