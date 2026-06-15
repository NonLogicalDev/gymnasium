---
date: 2026-06-15
status: complete
subject: dot-plans
---

# Goal

Move durable plans to `.agent-plans/` and update the skills and repo references that teach or assume the old path.

# Context

Plan files are agent-maintenance artifacts. Keeping them under `.agent-plans/` makes them available for continuity while reducing clutter in skill and repo directories.

# Decisions

- Use `.agent-plans/` at the same ownership boundary where visible plan directories were previously used.
- Migrate existing plan directories instead of leaving mixed conventions.
- Update `build-with-plans` tests because plan location is a behavior boundary.

# Implementation Steps

1. Move existing visible plan directories to `.agent-plans/`.
2. Update `build-with-plans` guidance, metadata, and tests to reference `.agent-plans/`.
3. Update repo docs and existing plan text that mention old paths.
4. Validate no old visible plan-directory references or directories remain.
5. Run skill validation and checkpoint the result.

# Learning Log

- Hidden plan directories preserve the same durable ownership model without making plan files prominent in normal directory listings.

# Work Log

- [x] 2026-06-15 10:55 - Created the `.agent-plans` migration plan.

# Unfinished Work

- [x] Move existing plan directories.
- [x] Update skill guidance, tests, metadata, and docs.
- [x] Validate references, skill shape, and git status.
- [x] Prepare final checkpoint and push.

# Validation

- Directory scan found no old visible plan directories.
- Hidden-path reference scan found no bare old-path references.
- `uv run --with pyyaml .../quick_validate.py .../build-with-plans` reported `Skill is valid!`.
