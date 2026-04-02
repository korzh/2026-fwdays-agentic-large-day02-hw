---
description: Security for Excalidraw — imports, rendering, collab, secrets, Firebase
alwaysApply: true
---

# Security (Excalidraw)

## Imported data and file format

- **`.excalidraw` / JSON / libraries**: parse with the same paths as existing code (`restore`, schema validation). Never `eval`, `new Function`, or deserialize untrusted code.
- **Clipboard and deep links**: treat payload as hostile; validate shape and size before merging into scene state.
- **Binary files (images)**: respect MIME checks and size limits; do not trust file extensions alone.

## XSS and rendering

- **SVG / canvas / DOM**: avoid injecting raw user strings into HTML or SVG without escaping or a safe pipeline already used in the editor.
- **`dangerouslySetInnerHTML`**: only with static or strictly sanitized content; prefer React text nodes or existing i18n patterns.
- **Third-party URLs**: validate schemes (`https` where required); avoid `javascript:` and opaque redirects.

## Collaboration and encryption

- **E2E / encryption** (`excalidraw-app` collab, encryption helpers): do not log keys, room IDs used as secrets, or decrypted payloads. Do not weaken IV usage, key derivation, or “skip verification” shortcuts for convenience.
- **WebSocket / sync payloads**: validate message types before applying to scene; keep reconciliation compatible with server expectations.

## Secrets and configuration

- **API keys / Firebase config**: load from env / build-time injection; never commit real keys. Document new env vars without embedding production values.
- **Firebase**: respect Storage and Firestore rules; client code must not assume server-side trust boundaries are optional.

## Content Security Policy

- If you change how scripts, workers, or blob URLs load: align with existing CSP headers and PWA behavior; avoid broadening `unsafe-inline` / wildcard origins without a strong reason.

## How to verify

1. **Imports**: open a crafted `.excalidraw` / library JSON in dev — scene should reject or sanitize invalid data without executing code or crashing the tab.
2. **UI**: paste untrusted text into labels and exported HTML/SVG paths; confirm no script execution in the browser (DevTools console clean of unexpected inline handlers).
3. **Collab**: inspect network logs — no encryption keys or raw decrypted content in clear text.
4. **Build**: `yarn test:code` and targeted tests under `excalidraw-app` / `packages/excalidraw` for changed areas; run the app locally and smoke-test import, export, and collab flows you touched.
