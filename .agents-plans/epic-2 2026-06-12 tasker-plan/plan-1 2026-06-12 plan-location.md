---
date: 2026-06-12
status: complete
subject: plan-location
---

# Goal

Record the earlier `build-with-plans` plan-location refinement and mark it as superseded by the current `$Tasker_Plan` repo-root layout.

# Context

This plan records the pre-`$Tasker_Plan` convention, where `.agent-plans/` lived at the owning unit. The current repo-owned layout is `.agents-plans/<EPIC_SLUG>/<PLAN_SLUG>.md` under the repo root, so this plan now lives in the `tasker-plan` epic.

# Decisions

- Historical decision: skill-owned work used `.agent-plans/` under the skill directory.
- Current decision: repo-owned work uses `.agents-plans/<EPIC_SLUG>/<PLAN_SLUG>.md`, so skill ownership is represented by epic subjects.
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
