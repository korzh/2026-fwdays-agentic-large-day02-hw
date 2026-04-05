# AGENTS.md

## Project Overview

This repository is the **Excalidraw monorepo**: a virtual whiteboard with a publishable React library (`@excalidraw/excalidraw`) and a full web app (`excalidraw-app`). Core logic lives in shared `packages/*`; the app wires collaboration, hosting, and product-specific UX. Agents and contributors should treat **[`docs/memory-bank.md`](docs/memory-bank.md)** as the place for fork-specific decisions and context—read it before non-trivial work and keep it aligned with the code.

## Project Structure

Excalidraw is a **monorepo** with a clear separation between the core library and the application:

- **`packages/excalidraw/`** - Main React component library published to npm as `@excalidraw/excalidraw`
- **`excalidraw-app/`** - Full-featured web application (excalidraw.com) that uses the library
- **`packages/`** - Core packages: `@excalidraw/common`, `@excalidraw/element`, `@excalidraw/math`, `@excalidraw/utils`
- **`examples/`** - Integration examples (NextJS, browser script)

## Tech Stack, Conventions

### Tech stack

- **Runtime / tooling**: Node.js, **Yarn** workspaces (`yarn@1.x`), **TypeScript** (strict)
- **UI**: **React** — `@excalidraw/excalidraw` declares peer React 17–19
- **Build**: **esbuild** for packages, **Vite** for the app
- **Tests**: **Vitest**, **jsdom**; formatting and lint via **Prettier** and **ESLint** (`@excalidraw/eslint-config`, `@excalidraw/prettier-config`)

### Conventions

- **Components**: functional components and hooks only; props type `{ComponentName}Props`; **named exports only** (no default exports); colocated tests `ComponentName.test.tsx`
- **TypeScript**: no `any`, no `@ts-ignore`; prefer `type` over `interface` for simple shapes; use `import type { ... }` for type-only imports
- **Files**: kebab-case for non-component files (e.g. `element-utils.ts`); **PascalCase** for component files (e.g. `LayerUI.tsx`)

Cursor rule **`.cursor/rules/conventions.mdc`** applies these conventions to `packages/**/*.ts` and `packages/**/*.tsx`.

## Constraints

### Protected files

Do **not** change the following without **explicit approval** and a plan that includes full dependency review, the full test suite, and manual QA:

- `packages/excalidraw/scene/renderer.ts` — render pipeline
- `packages/excalidraw/data/restore.ts` — file format compatibility
- `packages/excalidraw/actions/manager.ts` — action system
- `packages/excalidraw/types.ts` — core types

### Documentation and context

- After **meaningful** product or structural changes, update **[`docs/memory-bank.md`](docs/memory-bank.md)** (and supplementary `docs/*` files if used) per the pattern described there; stale docs are worse than missing docs.
- When Memory Bank text disagrees with the code, **the code wins** until the docs are corrected.

## Development Workflow

1. **Package Development**: Work in `packages/*` for editor features
2. **App Development**: Work in `excalidraw-app/` for app-specific features
3. **Testing**: Always run `yarn test:update` before committing
4. **Type Safety**: Use `yarn test:typecheck` to verify TypeScript

## Development Commands

```bash
yarn test:typecheck  # TypeScript type checking
yarn test:update     # Run all tests (with snapshot updates)
yarn fix             # Auto-fix formatting and linting issues
```

## Memory Bank

Persistent project context is defined in **[`docs/memory-bank.md`](docs/memory-bank.md)** (optional supplementary files under `docs/`). Keep the Memory Bank **accurate and updated after each meaningful project change** (features, refactors, tooling, architectural decisions).

## Architecture Notes

### Package System

- Uses Yarn workspaces for monorepo management
- Internal packages use path aliases (see `vitest.config.mts`)
- Build system uses esbuild for packages, Vite for the app
- TypeScript throughout with strict configuration
