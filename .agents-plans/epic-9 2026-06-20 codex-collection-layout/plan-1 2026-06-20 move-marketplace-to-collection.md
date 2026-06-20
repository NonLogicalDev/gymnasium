---
date: 2026-06-20
status: complete
subject: codex-collection-layout
---

# Goal

Move the Codex marketplace under `agents-codex/collection/` so `agents-codex/` can hold other Codex-specific material without being the marketplace root.

# Context

The Claude runtime tree already uses `agents-claude/collection/` as the marketplace root. Codex still had the marketplace manifest at `agents-codex/.agents/plugins/marketplace.json` and plugin sources at `agents-codex/plugins/`, which made the whole `agents-codex/` directory behave like the marketplace root.

# Decisions

- Make `agents-codex/collection/` the Codex marketplace root.
- Move both the Codex marketplace manifest and the Codex plugin source placeholder under `collection/`.
- Add `agents-codex/README.md` to describe the Codex runtime folder now that it can contain more than the marketplace.
- Preserve the user's existing README/AGENTS edits for the Claude runtime layout.

# Implementation Steps

1. Move `agents-codex/.agents/` to `agents-codex/collection/.agents/`.
2. Move `agents-codex/plugins/` to `agents-codex/collection/plugins/`.
3. Update root README and AGENTS references.
4. Add a Codex runtime README mirroring the collection-root convention.
5. Validate the marketplace JSON and collection filesystem structure.
6. Checkpoint the Gymnasium repo without staging unrelated untracked runtime work.

# Work Log

- [x] 2026-06-20 14:17 - Confirmed the Claude runtime already uses a `collection/` marketplace root.
- [x] 2026-06-20 14:17 - Moved Codex marketplace files under `agents-codex/collection/`.
- [x] 2026-06-20 14:17 - Updated docs and stale path notes.
- [x] 2026-06-20 14:20 - Validated marketplace structure.

# Unfinished Work

- [x] Move Codex marketplace root.
- [x] Update docs.
- [x] Validate marketplace structure.
- [x] Checkpoint the focused Gymnasium change.


# Validation

- `agents-codex/collection/.agents/plugins/marketplace.json` exists and parses as JSON.
- `agents-codex/collection/plugins/` exists as the Codex plugin source root.
- Marketplace validation checked non-empty `name`, `interface.displayName`, `plugins` as an array, and source-path containment for any future plugin entries.
- Current docs point Codex marketplace users at `agents-codex/collection/`; remaining old-path references are historical notes in plan files.
