---
date: 2026-06-12
status: in-progress
subject: git-committer
---

# Goal

Create a `git-committer` skill for writing crisp public-facing commit messages.

# Context

The skill should help agents write messages that respect reviewer time and avoid AI slop. A good message should use imperative tone and answer the relevant reviewer questions: context TL;DR, what changed, why it changed, and how it was validated when applicable.

# Decisions

- The skill will live at `agent-codex/skills/git-committer/`.
- The plan lives with the skill at `agent-codex/skills/git-committer/agent-plans/`.
- Abhinav's commit-message reference is useful source material, but this skill should be a concise public-facing commit-message authoring guide.

# Implementation Steps

- [ ] Initialize `git-committer` with the skill creator scaffold.
- [ ] Write `SKILL.md` with commit-message structure, quality rules, and anti-slop checks.
- [ ] Add OpenAI metadata.
- [ ] Add behavioral tests for public-facing commit-message quality.
- [ ] Validate the skill folder.
- [ ] Checkpoint the completed skill.

# Learning Log

- A baseline no-skill attempt produced a reasonable message, but it did not explicitly enforce all four reviewer questions as a reusable standard.

# Work Log

- [x] 2026-06-12 22:44 - Created the initial git-committer skill plan before scaffolding the skill.

# Unfinished Work

- [ ] Scaffold and author the skill.
- [ ] Validate and checkpoint the skill.
