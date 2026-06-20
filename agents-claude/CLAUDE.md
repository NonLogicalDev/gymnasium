# CLAUDE.md — Gymnasium (Claude runtime)

Curated guidance for Claude Code agents. The goal is to reduce papercuts and to
keep agents stable across long-running tasks so they can run further without
drifting or breaking down.

This file is a working draft. Add rules here as real sessions surface them, then
promote the durable ones into a published plugin under `plugins/`.

## How to use this file

- Drop snippets into a project's own `CLAUDE.md`, or install them via a plugin
  from the `gymnasium-claude` marketplace in `collection/`.
- Keep each rule short, imperative, and testable. Prefer one concrete rule over a
  paragraph of advice.
- Note *why* a rule exists when it is non-obvious, so future edits do not undo it.

## Staying stable over long tasks

- Re-read the task and any plan file before each major step; do not work from
  memory of the original prompt once context has been summarized.
- Maintain a durable plan/checklist on disk for multi-step work so progress
  survives context compaction.
- Verify each change before moving on (build, test, or run); never report done on
  an unverified step.
- Make small, reviewable edits over broad rewrites; commit/checkpoint at stable
  points.

## Avoiding common papercuts

- Use absolute paths and confirm the working directory before file operations.
- Prefer the dedicated file/search tools over shell `cat`/`sed`/`grep` equivalents.
- Read a file before editing it; match surrounding style, naming, and comment
  density.

## Conventions

- Claude-specific skills, agents, commands, and hooks live inside plugins under
  `collection/plugins/`; shared runtime-agnostic skills live in the repo's
  top-level `agents/skills/`.
