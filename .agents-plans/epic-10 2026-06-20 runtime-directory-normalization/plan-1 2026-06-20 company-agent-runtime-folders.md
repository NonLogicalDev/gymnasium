---
date: 2026-06-20
status: complete
subject: runtime-directory-normalization
---

# Goal

Rename runtime-specific marketplace folders to `agents-<company>-<agent>` and
add a Google Antigravity scaffold.

# Context

The repo is becoming a singular home for multiple agent marketplaces. The
previous names `agents-codex` and `agents-claude` were agent-only and became
ambiguous once Google entered the set.

# Decisions

- Use `agents-openai-codex` for Codex.
- Use `agents-anthropic-claude` for Claude Code.
- Use `agents-google-antigravity` for Google's current agent product surface.
- Do not create a fake Antigravity marketplace manifest; local `agy` help and
  validation confirm plugin directories with `plugin.json`, but not a collection
  manifest schema.

# Implementation Steps

1. Rename the Codex and Claude runtime directories.
2. Add the Google Antigravity scaffold.
3. Update repo docs and runtime docs.
4. Validate the resulting layout.
5. Checkpoint the repo.

# Work Log

- [x] 2026-06-20 14:33 - Renamed `agents-codex` to `agents-openai-codex`.
- [x] 2026-06-20 14:33 - Renamed `agents-claude` to `agents-anthropic-claude`.
- [x] 2026-06-20 14:33 - Added the initial `agents-google-antigravity` scaffold.
- [x] 2026-06-20 14:33 - Validated docs, plugin scaffold, and git status.

# Unfinished Work

- [x] Validate the resulting layout.
- [x] Checkpoint the repo.

# Validation

- Runtime-specific top-level folders now follow `agents-<company>-<agent>`.
- Codex and Claude marketplace JSON files parse successfully.
- Google Antigravity scaffold exists under `agents-google-antigravity/collection/plugins/`.
- Live README and AGENTS guidance points at the new runtime folder names.
