---
date: 2026-06-13
status: complete
subject: default-delegation
---

# Goal

Refine `delegator` so its default operating mode is manager-style delegation for any meaningful work, even when the work is not parallelizable.

# Context

The user clarified that the main thread should behave like a manager. It should protect context by delegating meaningful reads and writes by default, use one subagent when there is no useful parallel split, batch multiple subagents when independent work exists, and avoid micromanaging without a concrete reason.

# Decisions

- Preserve the skill's existing batching and integration guidance.
- Add an explicit default-delegation invariant near the top of the skill.
- Make "single subagent" a first-class path for meaningful non-parallel work.
- Tighten local-work exceptions so they cover coordination, integration, approvals, and trivial operations rather than normal meaningful reads/writes.
- Add a durable scenario for meaningful single-thread work.

# Implementation Steps

1. Capture baseline current-skill behavior with a fresh subagent.
2. Patch `SKILL.md` to state the default delegation invariant.
3. Add a reusable behavioral scenario for meaningful one-file work.
4. Run mechanical validation and pressure-test the updated scenario.
5. Checkpoint the completed refinement.

# Learning Log

- Baseline against the current skill already delegated a meaningful one-file edit to one worker, but it relied on inference from "Do not make the main thread the default bulk worker." The skill should make that behavior explicit.
- The key boundary is not "parallelizable vs local"; it is "meaningful work vs coordination/integration." Parallelization determines how many subagents to spawn, not whether to delegate.
- Pressure test passed for meaningful non-parallel work: the revised skill chose one focused subagent, not main-thread work or fake parallelism.
- Adjacent valid case passed: the revised skill kept a trivial git-status-style coordination check local and did not spawn a subagent.

# Work Log

- [x] 2026-06-13 00:11 - Read current `delegator` guidance and repo-local skill update conventions.
- [x] 2026-06-13 00:11 - Ran baseline current-skill scenario for a meaningful one-file edit.
- [x] 2026-06-13 00:11 - Created this refinement plan.
- [x] 2026-06-13 00:12 - Patched `SKILL.md` and reusable scenarios for default delegation.
- [x] 2026-06-13 00:12 - Ran `quick_validate.py`; the skill is mechanically valid.
- [x] 2026-06-13 00:12 - Ran two fresh pressure checks against the revised skill.

# Unfinished Work

None.
