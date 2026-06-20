---
date: 2026-06-18
status: in-progress
subject: rename-alignment
---

# Goal

Align the renamed skill folders under `.agents/skills/` with their internal metadata, references, and supporting structure so the new chat-facing names are internally coherent.

# Context

The old canonical folders such as `delegator/` and `build-with-plans/` were replaced by chat-facing folders like `Tasker_Delegate/` and `Tasker_Plan/`. The content inside those folders still largely referred to the old canonical names, and the vault also expected some helpers in the old shared locations.

# Product Integration

- Existing product model: skill identity is split between folder names, `SKILL.md` frontmatter, test fixtures, agent prompts, and helper-script paths.
- New requirement's real intent: make the renamed folders feel like the real public identity instead of a thin wrapper over the older canonical names.
- Cleanest integrated model: each skill folder should expose one consistent primary name, while compatibility references stay only where they are needed for runtime behavior.
- Existing pieces that should move, change, or disappear: stale canonical-name references inside the renamed folders should be rewritten or explicitly framed as compatibility aliases; shared helper paths should either exist or stop being documented.
- Architecture impact: low; this is a metadata and layout cleanup across the skill set.
- Why this is better than a local patch: it avoids having folder names, skill names, and instructions disagree in ways that confuse future editing and invocation.

# Implementation Steps

- Audit each renamed skill for mismatches between folder name, frontmatter `name`, headings, test text, and helper references.
- Update the smallest set of files needed to make the renamed identity coherent.
- Restore or rewire any shared helper paths that the skills still document.
- Run a mechanical verification pass over the resulting tree.

# Work Log

- [x] 2026-06-18 22:16 - Created a cross-skill cleanup plan after confirming the rename happened in the external `agents-codex` repo and is exposed into the vault through a symlink.
- [x] 2026-06-18 22:16 - Patched the renamed skills so folder identity, metadata, prompts, tests, and the QMD helper path agree with the new chat-facing names.
- [x] 2026-06-18 22:16 - Verified the resulting tree with targeted ripgrep sweeps for stale canonical handles and spot-checked the updated frontmatter and agent metadata.
- [x] 2026-06-18 22:19 - Switched the skill `name` fields to the requested `NAMESPACE_NAME` form and updated the identity text to prefer the exact chat-facing names over validator-style lowercase aliases.

# Unfinished Work

- [ ] Decide whether the vault should also expose a compatibility wrapper again at `.agents/scripts/qmd` or rely only on the skill-local wrapper path.
