---
date: 2026-06-20
status: complete
subject: plan-layout-consistency
---

# Goal

Move all in-repo Gymnasium plan files into the current `$Tasker_Plan` repo-root layout and rename epics and plans to the current slug standard.

# Context

The repo still used the previous `.agent-plans` convention, including skill-local plan directories. The current `$Tasker_Plan` contract allows repo-owned plans only under `.agents-plans/<EPIC_SLUG>/<PLAN_SLUG>.md`, with dated `epic-<N>` and `plan-<N>` slugs.

# Decisions

- Use the repo-root layout because this cleanup is owned by the Gymnasium checkout.
- Assign migrated epic numbers by historical plan date and ownership path, then add this migration as the next epic.
- Preserve existing plan statuses and unfinished-work items; do not mark unrelated work complete during the move.
- Patch repo documentation paths from `agent-codex` to the actual `agents-codex` tree while doing the plan-path consistency pass.

# Implementation Steps

1. Create the new `.agents-plans/` epic layout.
2. Move every tracked plan file with `git mv`.
3. Rename plan basenames to `plan-<N> YYYY-MM-DD <subject>.md`.
4. Update stale path guidance in migrated plans and repo docs.
5. Verify no `.agent-plans` directories remain and no tracked plan file violates the current layout.
6. Checkpoint the completed migration.

# Learning Log

- The previous skill-local `.agent-plans` convention conflicted with the current `$Tasker_Plan` repo-root layout, so preserving ownership now means naming epics by ownership rather than nesting plan directories under each skill.

# Work Log

- [x] 2026-06-20 13:56 - Inventoried all old `.agent-plans` files and chose the repo-root `$Tasker_Plan` layout.
- [x] 2026-06-20 13:56 - Moved and renamed the tracked plan files into `.agents-plans/`.
- [x] 2026-06-20 13:56 - Patched stale plan-location wording and repo path docs.
- [x] 2026-06-20 13:59 - Ran final hidden-path and tracked-layout validation.

# Unfinished Work

- [x] Move and rename old plan files.
- [x] Update stale path references that teach the old convention as current.
- [x] Validate the resulting layout.
- [x] Checkpoint the repo.


# Validation

- No `.agent-plans`, `agent-plans`, or `agents-plans` directories remain outside the allowed `.agents-plans/` container.
- All 9 plan files match `epic-<N> YYYY-MM-DD <subject>/plan-<N> YYYY-MM-DD <subject>.md`.
- No tracked files remain under old `.agent-plans/`, `agent-plans/`, or `agents-plans/` paths.
- `$Tasker_Plan` path examples now include `.md` and distinguish repo-root `.agents-plans/<EPIC_SLUG>/` from brain-project `agents-plans/`.
