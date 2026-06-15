---
date: 2026-06-12
status: complete
subject: git-committer
---

# Goal

Create a `git-committer` skill for writing crisp public-facing commit messages.

# Context

The skill should help agents write messages that respect reviewer time and avoid AI slop. A good message should use imperative tone and answer the relevant reviewer questions: context TL;DR, what changed, why it changed, and how it was validated when applicable.

# Decisions

- The skill will live at `agent-codex/skills/git-committer/`.
- The plan lives with the skill at `agent-codex/skills/git-committer/.agent-plans/`.
- Abhinav's commit-message reference is useful source material, but this skill should be a concise public-facing commit-message authoring guide.

# Implementation Steps

- [x] Initialize `git-committer` with the skill creator scaffold.
- [x] Write `SKILL.md` with commit-message structure, quality rules, and anti-slop checks.
- [x] Add OpenAI metadata.
- [x] Add behavioral tests for public-facing commit-message quality.
- [x] Validate the skill folder.
- [x] Checkpoint the completed skill.

# Learning Log

- A baseline no-skill attempt produced a reasonable message, but it did not explicitly enforce all four reviewer questions as a reusable standard.
- First pressure test with the skill still repeated file inventory and lint. The skill needed the user-pressure boundary earlier in the output contract, not only in the anti-slop section.
- Second pressure test still produced shorter file inventory and `Verified with lint.` The skill needs exact invalid patterns and replacement questions, not only abstract principles.
- Third pressure test removed slop but returned a subject only. The skill needs to state that "commit message only" excludes commentary, not the required body.
- Fourth pressure test returned a good subject/body but still laundered helper and fixture inventory into the body. The skill needs to require behavior-level validation language, not support-artifact language.
- Fifth pressure test regressed into a bullet-list inventory. The skill needs a high-priority conflict rule and a bullet-list ban for routine process details.
- Sixth pressure test still mentioned parser handling, helper utilities, fixtures, and lint. The rule needs an explicit pre-return scan for routine-inventory words.
- Follow-up format decision: public commit messages should first state what changed, then use `## Context` and `## Validation` headings.

# Work Log

- [x] 2026-06-12 22:44 - Created the initial git-committer skill plan before scaffolding the skill.
- [x] 2026-06-12 22:50 - Authored `SKILL.md`, OpenAI metadata, and behavioral scenarios.
- [x] 2026-06-12 22:50 - Validated the `git-committer` skill folder.
- [x] 2026-06-12 22:51 - Captured pressure-test failure: the draft repeated parser/helper/fixture/lint inventory.
- [x] 2026-06-12 22:51 - Tightened the output contract to treat file and routine-check requests as handoff material unless they change the reviewed contract.
- [x] 2026-06-12 22:52 - Captured second pressure-test failure: shorter inventory plus `Verified with lint.`
- [x] 2026-06-12 22:52 - Added invalid phrase patterns and reviewer-value replacement prompts.
- [x] 2026-06-12 22:53 - Captured third pressure-test failure: subject-only output.
- [x] 2026-06-12 22:53 - Added a mandatory-body clarification and reusable subject-only regression scenario.
- [x] 2026-06-12 22:54 - Captured fourth pressure-test residue: helper and fixture wording remained in the body.
- [x] 2026-06-12 22:54 - Tightened validation language to describe behavior proven, not helper or fixture artifacts.
- [x] 2026-06-12 22:55 - Captured fifth pressure-test failure: bullet-list inventory for parser/helpers/fixtures/lint.
- [x] 2026-06-12 22:55 - Added a conflict rule and banned bullet-list inventories for routine process details.
- [x] 2026-06-12 22:56 - Captured sixth pressure-test failure: routine inventory persisted in prose.
- [x] 2026-06-12 22:56 - Added hard rules and a pre-return scan for routine-inventory words.
- [x] 2026-06-12 22:57 - Final pressure test passed with a subject/body message and no routine inventory.
- [x] 2026-06-12 23:01 - Updated the message shape to put what changed first, followed by `## Context` and `## Validation`.
- [x] 2026-06-12 23:01 - Verified the revised shape with a fresh behavior check for the `git-committer` skill commit.

# Unfinished Work

- None.
