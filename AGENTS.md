# AGENTS.md - Gymnasium

This repo curates reusable skills and plugins for coding agents.

## Scope

- `agents/skills/` contains shared, runtime-agnostic skills.
- `agents-codex/` contains Codex-specific guidance, plugin collections, and runtime material.
- `agents-codex/collection/` contains the Codex marketplace manifest and plugin sources.
- `agents-claude/` contains Claude-specific guidance, plugins, and skills.
- Future agent runtimes should get their own top-level `agents-<runtime>/` folder.

## Working Rules

- Preserve upstream skill/plugin directories when importing them.
- Prefer small, reviewable edits over broad rewrites.
- Keep runtime-specific conventions inside that runtime's folder.
- Do not add generated caches, logs, or local secrets.

## Git

This repo starts with an empty `INIT` commit. Keep later commits focused on one import or change at a time.

