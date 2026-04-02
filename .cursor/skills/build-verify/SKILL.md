---
name: build-verify
description: Builds the Excalidraw monorepo and runs verification (TypeScript, ESLint, Prettier, Vitest). Use when the user asks to build, verify, validate changes, run tests before commit, or match CI; after substantive edits in packages/excalidraw, excalidraw-app, or packages/*.
---

# Build & verify (Excalidraw monorepo)

Run commands from the **repository root** with **Yarn 1** (`packageManager` in root `package.json`). Requires **Node >= 18**.

## When to build what

| Changed area | Build step |
| --- | --- |
| `packages/common`, `packages/math`, `packages/element`, `packages/excalidraw` | `yarn build:packages` (order: common → math → element → excalidraw) |
| `excalidraw-app/` or verifying a full production bundle | `yarn build` (Vite app build) |

For many edits, `yarn test:typecheck` and Vitest already exercise TypeScript; run package/app builds when validating publishable output or app bundle.

## Verification tiers

**1. Fast checks (typical after edits)**

```bash
yarn test:typecheck
yarn test:app --watch=false
```

**2. Pre-commit workflow** (see `AGENTS.md`)

```bash
yarn test:typecheck
yarn test:update
```

Use `yarn fix` first if formatting or lint may fail (`yarn fix` runs Prettier write + ESLint fix).

**3. Full gate (closest to `test:all`)**

```bash
yarn test:all
```

Runs: `test:typecheck` → ESLint → Prettier `--list-different` → Vitest once. Does **not** update snapshots; fix snapshot drift with `yarn test:update` when intended.

## Reference (root scripts)

| Script                | Role                                    |
| --------------------- | --------------------------------------- |
| `yarn build:packages` | ESM builds for all library packages     |
| `yarn build`          | `excalidraw-app` production build       |
| `yarn test:typecheck` | `tsc` project-wide                      |
| `yarn test:update`    | Vitest with snapshot updates, no watch  |
| `yarn fix`            | Prettier write + ESLint `--fix`         |
| `yarn test:all`       | Typecheck + lint + format check + tests |

## Agent behavior

- **Execute** these commands in the project root; do not only suggest them.
- If a step fails, read the output, fix the underlying issue, then re-run the failed step or the minimal tier that covers the fix.
- After meaningful workflow or script changes, align `AGENTS.md` / `docs/memory-bank.md` if the project tracks them there.
