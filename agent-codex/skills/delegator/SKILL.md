---
name: delegator
description: Use when work has multiple independent tracks, broad repo exploration, audits, research, validation, implementation slices, large logs, or any task likely to consume main-thread context; use when the main thread should coordinate with subagents, preserve project context, avoid blocking, or batch parallel work.
---

# Delegator

## Overview

Keep the main thread lightweight. Its context is reserved for coordination, project memory, user communication, integration decisions, and checkpoints. Push bulk work to focused subagents whenever independent work can proceed without blocking the next local step.

## Core Rule

Before reading deeply or starting broad work, split the task:

1. Identify the immediate critical-path action the main thread should do locally.
2. Identify independent sidecar tasks that can run without that result.
3. If there are two or more sidecars, dispatch them in one batch.
4. While they run, continue meaningful non-overlapping work locally.

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
- Make safe edits that do not overlap delegated scopes.
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

- The task is tiny and can be completed with one or two quick commands.
- The next step is an immediate blocker and delegation would leave the main thread idle.
- The work requires sensitive secrets, approvals, or destructive operations.
- Shared state is so tight that separate agents would conflict.
- The user explicitly forbids subagents.

Even then, keep the main thread narrow. If the local task grows, split it and delegate the new independent parts.

## Red Flags

| Red flag | Correct action |
| --- | --- |
| "I'll just inspect everything here first." | Dispatch explorers before deep reads. |
| "Coordinating agents is overhead." | Use one small batch with bounded prompts. |
| "I'll wait for the first agent before deciding." | Batch obvious independent tracks now. |
| "I'll wait while agents work." | Continue non-overlapping coordination or safe local work. |
| "One worker can handle all modules." | Split by ownership when edits are independent. |
| "The agent says it passed." | Review output and run integrated validation. |

## Tests

When changing this skill,
read [tests/README.md](tests/README.md).
Run the relevant scenarios with fresh subagents that have empty context windows.
