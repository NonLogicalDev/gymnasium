---
date: 2026-06-12
status: complete
subject: plan-location
---

# Goal

Refine `build-with-plans` so `.agent-plans` live at the smallest durable operating unit, especially under a skill directory when the task is skill-owned.

# Context

The current skill should default to `.agent-plans/` at the owning unit. For this repo, the useful operating unit is often a single skill under `agent-codex/skills/`, so plans should live with the skill they govern. Cross-cutting work can still use root task directories like `.agent-plans/YYYY-MM-DD-task/`.

# Decisions

- Skill-owned work should use `.agent-plans/` under the skill directory.
- Cross-cutting repo work can use root `.agent-plans/YYYY-MM-DD-task/`.
- Existing active plans should be updated when they clearly own a follow-up.

# Implementation Steps

- [x] Update `build-with-plans` plan-location guidance.
- [x] Add a behavioral scenario for skill-local planning.
- [x] Validate the skill folder.
- [x] Checkpoint the planning refinement.

# Learning Log

- Plan placement is part of the skill's behavioral contract, not just a repo organization detail.

# Work Log

- [x] 2026-06-12 22:44 - Created the plan-location refinement plan before changing the skill.
- [x] 2026-06-12 22:44 - Updated the plan-location rule and added a skill-local planning scenario.
- [x] 2026-06-12 22:44 - Validated `build-with-plans` after the refinement.

# Unfinished Work

- None.
