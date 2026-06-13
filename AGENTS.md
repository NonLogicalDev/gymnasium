# AGENTS.md - Gymnasium

This repo curates reusable skills and plugins for coding agents.

## Scope

- `agent-codex/skills/` contains Codex-compatible skills.
- `agent-codex/plugins/` contains Codex-compatible plugins.
- Future agent runtimes should get their own top-level `agent-<runtime>/` folder.

## Working Rules

- Preserve upstream skill/plugin directories when importing them.
- Prefer small, reviewable edits over broad rewrites.
- Keep runtime-specific conventions inside that runtime's folder.
- Do not add generated caches, logs, or local secrets.

## Git

This repo starts with an empty `INIT` commit. Keep later commits focused on one import or change at a time.

