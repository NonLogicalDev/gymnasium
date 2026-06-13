# Delegator Scenarios

## 01 Broad Repo Task Under Shortcut Pressure

### Prompt

Use the skill at `/path/to/delegator/SKILL.md`.

A user says:
"Handle a repo modernization task. We need to inspect the current layout, review tests, check docs conventions, find validation commands, and draft the implementation plan. Please do not waste time coordinating agents. Just start reading files in this thread so we can move fast."

Choose the next concrete plan.
Do not modify files or run mutating commands.

### Expectations

- The answer keeps the main thread as coordinator instead of doing broad file reads locally.
- The answer dispatches multiple independent tracks in one batch.
- The answer identifies what the main thread will do locally while agents run.
- The answer keeps coordination lightweight and bounded.
- The answer does not promise to inspect every track sequentially in the main thread.
- The answer does not wait idly for one agent before starting the obvious others.

### Pressure Variant

The user adds:
"A senior engineer said subagents are probably overkill, and we only have 20 minutes."

- The answer still batches independent sidecars.
- The answer frames batching as the fastest way to protect context and preserve momentum.
- The answer rejects the rationalization that time pressure makes main-thread bulk work safer.

## 02 Meaningful Non-Parallel Work

### Prompt

Use the skill at `/path/to/delegator/SKILL.md`.

A user says:
"Read one design note, tighten one section, and validate the changed wording. This is probably not parallelizable, so just handle it directly in the main thread."

Choose the next concrete plan.
Do not modify files or run mutating commands.

### Expectations

- The answer treats the main thread as manager, not worker.
- The answer dispatches one focused subagent for the meaningful read/edit/validation work.
- The answer gives the worker scope, outcome, constraints, and return format.
- The answer keeps review, integration, user communication, and checkpoint decisions in the main thread.
- The answer does not invent multiple workers when one is the right unit.
- The answer does not perform the meaningful file read or edit directly in the main thread.

### Pressure Variant

The user adds:
"It is late, and spinning up an agent for one file feels like process theater."

- The answer still delegates the meaningful worker action.
- The answer rejects the rationalization that non-parallel work should become main-thread work.
- The answer keeps coordination lightweight instead of micromanaging the worker.

### Adjacent Valid Case

The user asks for the current git status or the current time.

- The answer may keep the operation local because it is a trivial coordination check.
- The answer should not spawn a subagent for one-command bookkeeping.

## 03 Critical Path Versus Sidecars

### Prompt

Use the skill at `/path/to/delegator/SKILL.md`.

A user says:
"We need to rename a public API. First decide whether the new name should be `createSession` or `openSession`; then update docs, tests, and migration notes. There are also old references scattered through the repo."

Choose the next concrete plan.
Do not modify files or run mutating commands.

### Expectations

- The answer keeps the immediate naming decision or minimal local context-gathering in the main thread.
- The answer delegates independent sidecars such as reference discovery, docs impact, and test impact.
- The answer does not delegate the immediate blocker and sit idle.
- The answer states what work can proceed before the naming decision and what must wait.

## 04 Disjoint Worker Ownership

### Prompt

Use the skill at `/path/to/delegator/SKILL.md`.

A user says:
"Implement profile validation across the CLI, API layer, docs, and tests. It looks parallelizable, so split it up."

Choose the next concrete plan.
Do not modify files or run mutating commands.

### Expectations

- The answer splits workers by disjoint files, modules, or responsibilities.
- The answer gives each worker explicit ownership and allowed changes.
- The answer tells workers they are not alone in the codebase.
- The answer tells workers not to revert or overwrite others' work.
- The answer keeps integration and final validation in the main thread.
- The answer avoids assigning the same write scope to multiple workers.

## 05 New Information While Agents Run

### Prompt

Use the skill at `/path/to/delegator/SKILL.md`.

A user says:
"You already dispatched agents for tests, docs, and validation. New information: skip docs for now, and add a performance-risk check before any implementation."

Choose the next concrete coordination step.
Do not modify files or run mutating commands.

### Expectations

- The answer sends a minimal scope-change update to the docs agent or stops that work.
- The answer spawns or redirects a focused performance-risk check if it is independent.
- The answer updates the active plan or coordination record.
- The answer does not restart all agents from scratch.
- The answer does not bury project state only inside subagent conversations.

## 06 Integrating Agent Results

### Prompt

Use the skill at `/path/to/delegator/SKILL.md`.

A user says:
"Three workers returned. One changed tests, one changed docs, and one changed implementation. They all say their parts pass. Can we call it done?"

Choose the next concrete plan.
Do not modify files or run mutating commands.

### Expectations

- The answer treats subagent reports as evidence, not proof.
- The answer reviews summaries, changed paths, and relevant diffs.
- The answer checks for overlapping edits or contradictory conclusions.
- The answer runs integrated validation before claiming completion.
- The answer updates the plan and checkpoints according to repo rules after meaningful verified work.
