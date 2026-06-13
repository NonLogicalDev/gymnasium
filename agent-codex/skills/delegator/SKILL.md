---
name: delegator
description: Use by default for meaningful agent work, including reads, writes, repo exploration, audits, research, validation, implementation slices, large logs, or tasks likely to consume main-thread context; use when the main thread should manage subagents, preserve project context, avoid blocking, or batch parallel work.
---

# Delegator

## Overview

Keep the main thread lightweight. Its context is reserved for coordination, project memory, user communication, integration decisions, and checkpoints. Delegate meaningful work by default, then integrate the results deliberately.

## Core Rule

The main thread is the manager, not the default worker. It should not personally perform meaningful reads, searches, edits, audits, or validation when a focused subagent can do that work.

Before starting work, split the task:

1. Identify the immediate critical-path action the main thread should do locally.
2. Identify meaningful work that should be delegated.
3. If there is one meaningful work item, dispatch one focused subagent.
4. If there are two or more independent work items, dispatch them in one batch.
5. Keep only coordination, integration, approvals, and narrow context tracking local.

Parallelization decides how many subagents to spawn, not whether to delegate.

## Manager Mode

Use one subagent for meaningful non-parallel work:

- Reading one important file and summarizing it.
- Editing one bounded section.
- Running one focused validation command.
- Inspecting one failure or log.

Use multiple subagents when work can split cleanly.
Do not manufacture parallelism for its own sake,
but do delegate the worker action.

Avoid micromanaging:

- Give the worker a clear outcome, scope, constraints, and return format.
- Let the worker choose tactical reads or edits inside that scope.
- Ask for changed paths, important evidence, validation, and blockers.
- Do not follow every worker step in the main thread unless risk or ambiguity requires it.

## Batch Rule

When batching is available:

1. Identify independent sidecar tasks that can run without the immediate local result.
2. If there are two or more sidecars, dispatch them in one batch.
3. Keep one local coordination task moving while they run.

Do not make the main thread the default bulk worker. Use subagents liberally for exploration, audits, comparisons, test investigation, validation, and disjoint implementation slices.

## Ownership

Main thread owns:

- User-facing updates and clarification.
- Active plan, project context, repo status, and checkpoint decisions.
- Decomposition, prioritization, and conflict resolution.
- Approval handling, destructive-operation decisions, and secret-sensitive work.
- Integrating reports, reviewing diffs, running final validation, and final response.

Subagents own:

- Focused repo exploration, source reads, searches, log review, and summarization.
- Independent research tracks and comparison work.
- Isolated test failures, validation passes, and risk checks.
- Bounded implementation work with disjoint write scopes.
- Drafts or patch sketches when shared edits would conflict.

## Dispatch Pattern

Use a compact packet for each agent:

```text
Objective:
Repo/path:
Scope:
Allowed changes:
Constraints:
Context:
Return format:
```

For coding workers, include:

- "You are not alone in the codebase."
- "Do not revert or overwrite changes made by others."
- "Own only these files/modules: ..."
- "List changed paths and validation commands in your final answer."

For explorers, ask for concise conclusions, relevant paths, commands run, and open questions. Do not ask for raw logs unless exact failure text matters.

## Batch First

Prefer a first wave of 2-5 agents when the task has independent tracks. Examples:

| Task shape | Batch |
| --- | --- |
| Broad repo orientation | Layout explorer, tests explorer, docs explorer, validation explorer |
| Multiple failing areas | One investigator per failure domain |
| Skill authoring | Baseline tester, convention explorer, pressure-test reviewer |
| Migration | Source behavior explorer, target API explorer, test impact explorer |
| Implementation with clear ownership | One worker per disjoint module or file group |

Do not dispatch one agent, wait, then decide whether to dispatch the obvious others. Serial delegation wastes the main thread and loses the point of delegation.

## Keep Moving

After dispatching, the main thread should work on non-overlapping critical-path tasks:

- Create or update the plan.
- Read repo-local instructions.
- Prepare the integration checklist.
- Apply narrow integration edits after reviewing worker output.
- Handle approvals or user updates.

Wait only when the next action truly depends on an agent result. Do not redo delegated work in the main thread.

## Route New Information

When new information arrives:

- Send a minimal delta to the affected existing agent if it changes that agent's task.
- Spawn a new focused agent if the new work is independent.
- Stop or redirect stale delegated work when scope changes.
- Update the active plan with the decision and current ownership.

Keep the main thread as the coordinator of record; do not let agent conversations become the only place where project state exists.

## Integrate Deliberately

When agents return:

1. Read the summary and changed-path list.
2. Check for overlapping edits or contradictory conclusions.
3. Inspect relevant diffs before accepting worker output.
4. Run final validation from the integrated workspace.
5. Update the plan and checkpoint according to repo rules.
6. Close completed agents that are no longer needed.

Subagent reports are evidence, not proof. The main thread still owns the final result.

## When To Keep Work Local

Keep the work local when:

- The task is trivial bookkeeping or a one-command coordination check.
- The next step is coordinator-only context tracking or integration review.
- The next step is an immediate blocker and delegating it would leave the main thread idle with no useful coordination work.
- The work requires sensitive secrets, approvals, or destructive operations.
- Shared state is so tight that separate agents would conflict.
- The user explicitly forbids subagents.

Even then, keep the main thread narrow. If a local task becomes meaningful worker work, delegate it.

## Red Flags

| Red flag | Correct action |
| --- | --- |
| "I'll just inspect everything here first." | Dispatch explorers before deep reads. |
| "This is not parallelizable, so I'll do it locally." | Dispatch one focused subagent for meaningful work. |
| "It is only one file, so the main thread can edit it." | Delegate meaningful edits; keep review and integration local. |
| "Coordinating agents is overhead." | Use one small batch with bounded prompts. |
| "I'll wait for the first agent before deciding." | Batch obvious independent tracks now. |
| "I'll wait while agents work." | Continue non-overlapping coordination or safe local work. |
| "One worker can handle all modules." | Split by ownership when edits are independent. |
| "The agent says it passed." | Review output and run integrated validation. |

## Tests

When changing this skill,
read [tests/README.md](tests/README.md).
Run the relevant scenarios with fresh subagents that have empty context windows.
