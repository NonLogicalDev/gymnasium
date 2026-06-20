---
date: 2026-06-20
status: complete
subject: agents-directory-alignment
---

# Goal

Move Gymnasium skills from `agents-codex/skills` into `agents/skills` and update `~/brain/.agents/skills` to point at the new location.

# Context

The repo now exposes skill names directly as the public `Tasker_*`, `Code_*`, `Search_QMD`, and `Skill_Harden` folders. The current brain symlink still points at `agents-codex/skills`, which makes the old runtime-specific folder name part of the active integration surface.

# Decisions

- Use `agents/skills` as the active skill root.
- Historical decision: leave `agents-codex/.agents/plugins/marketplace.json` and `agents-codex/plugins/` in place because that request only moved the skills root.
- Current follow-up: move the Codex marketplace root under `agents-codex/collection/`.
- Update current repo docs and current-path notes to `agents/skills`; keep older historical mentions only when they describe prior names or upstream source paths.
- Update the brain symlink directly, but do not checkpoint the vault because `.agents` already has unrelated dirty state.

# Implementation Steps

1. Move `agents-codex/skills` to `agents/skills` with git rename semantics.
2. Update README, AGENTS, NOTICE, and current-path plan notes.
3. Repoint `/Users/nonlogical/brain/.agents/skills` to `/Users/nonlogical/Projects/local/gymnasium/agents/skills`.
4. Verify the symlink target, repo status, and absence of live `agents-codex/skills` references.
5. Checkpoint the Gymnasium repo.

# Work Log

- [x] 2026-06-20 14:03 - Verified `~/brain` resolves to `/Users/nonlogical/Notes/Personal-OBS` and `.agents/skills` points at the old Gymnasium skills root.
- [x] 2026-06-20 14:03 - Moved the skill root and patched live references.
- [x] 2026-06-20 14:03 - Repointed the brain `.agents/skills` symlink.
- [x] 2026-06-20 14:06 - Ran final validation.
- [x] 2026-06-20 14:42 - Consolidated this plan under the shared `repo-organization` epic.

# Unfinished Work

- [x] Move skill root.
- [x] Update brain symlink.
- [x] Validate layout and references.
- [x] Checkpoint Gymnasium repo.


# Validation

- `/Users/nonlogical/brain/.agents/skills` resolves to `/Users/nonlogical/Projects/local/gymnasium/agents/skills`.
- `agents/skills/` contains the expected skill directories: `Code_Checkpoint`, `Code_Commit`, `Code_Develop`, `Code_Projects`, `Search_QMD`, `Skill_Harden`, `Tasker_Delegate`, and `Tasker_Plan`.
- Current docs use `agents/skills` for skill paths; remaining `agents-codex/skills` hits are limited to this migration plan describing the move.
- The vault `.agents` tree already had unrelated dirty state before the symlink update, so the vault was not checkpointed.
