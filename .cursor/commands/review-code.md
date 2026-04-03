# Code review

You are performing a **thorough code review** for this Excalidraw monorepo. The user invoked `/review-code`; treat their message, @-references, selections, and open editors as the **review scope**. If nothing is specified, infer scope from the most recent edits or ask briefly what to focus on.

## Project context (apply when relevant)

- Read **`AGENTS.md`** for structure, build commands (`yarn test:typecheck`, `yarn test:update`, `yarn fix`), and workflow.
- For non-trivial or architectural feedback, skim **`docs/memory-bank.md`** (and linked supplements) so suggestions align with documented decisions.
- Respect **protected files** in `.cursor/rules/do-not-touch.mdc`: do not suggest casual edits to `packages/excalidraw/scene/renderer.ts`, `packages/excalidraw/data/restore.ts`, `packages/excalidraw/actions/manager.ts`, or `packages/excalidraw/types.ts` without calling out the extra risk and verification burden.

## What to review

Cover as much as the scope allows:

1. **Correctness** — Logic bugs, edge cases, error handling, async/race issues, null/undefined safety, type soundness.
2. **Design** — Fit with existing patterns in the same package, API surface, coupling, duplication, naming consistency.
3. **Performance** — Hot paths, unnecessary re-renders, allocations, large dependencies in tight loops (when applicable).
4. **Security & privacy** — Unsafe HTML/DOM, injection, secrets, unsafe URLs, trust boundaries.
5. **Tests & maintainability** — Missing coverage for new behavior, brittle tests, unclear structure.
6. **Repo hygiene** — Lint/type issues implied by the change, snapshot or formatting expectations per `AGENTS.md`.

## How to report

Use this structure (adapt headings if the scope is tiny):

### Summary

2–4 sentences: what changed (or what you reviewed) and overall quality.

### Strengths

Bullet list of what is already solid.

### Issues and risks

Table or bullets with **severity** (`blocker` / `major` / `minor` / `nit`). Each item: **what**, **where** (file path + symbol or line range if known), **why it matters**, **concrete fix** (short).

### Suggested improvements

Ordered by impact. Prefer small, incremental changes. Distinguish **must-fix** from **nice-to-have**.

### Verification

What the author should run or check (e.g. `yarn test:typecheck`, targeted tests, manual QA) for this change.

## Citation rules

When pointing at existing code, use Cursor-style fenced citations so the user can jump to the code:

```startLine:endLine:path/to/file.ts
// excerpt or …
```

Do not dump entire files. Quote only the minimum needed.

## Tone

Be direct and specific; avoid vague praise. Every **major** or **blocker** item should include a actionable recommendation. If you lack context (e.g. intent of a PR), state assumptions explicitly.
