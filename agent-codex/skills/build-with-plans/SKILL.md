---
name: build-with-plans
description: Maintain numbered agent-plans while building. Use when the user invokes $BuildWithPlans, asks to add a task or plan, starts medium-sized design or functionality work, or works in a project with an agent-plans directory and wants durable decisions, timestamped work logs, unfinished work, and frequent git checkpoints.
---

# BuildWithPlans

Use this skill to keep implementation work durable across sessions. It turns medium-sized design or functionality changes into numbered plan files that capture decisions, learnings, work logs, and unfinished work while the code evolves.

The user-facing invocation is `$BuildWithPlans`. The validated skill name is `build-with-plans`.

## Core Rules

- Read the repo-local `AGENTS.md` before creating or editing plans. Repo instructions override this skill when they are more specific.
- Use `agent-plans/` under the project root unless the repo explicitly uses another location.
- Create or update a plan for every medium-sized design or functionality change.
- Do not create a new plan for tiny one-off edits unless the user asks.
- When a follow-up request belongs to the current active plan, update that existing plan instead of creating a new one.
- Create a new plan when the work changes scope, ownership boundary, or implementation strategy.
- Keep plans concise but operational: future sessions should understand what was decided, what changed, and what remains.
- Checkpoint often in git according to repo rules, especially before risky edits, after verified milestones, before switching context, and before ending a session with meaningful changes.

## Plan Naming

Use the next numeric plan id already present in `agent-plans/`:

```text
agent-plans/plan-NNN short-subject.md
```

Examples:

```text
agent-plans/plan-007 table-polish.md
agent-plans/plan-008 app-icon.md
agent-plans/plan-009 scan-excludes.md
```

Keep the subject short, lowercase, and descriptive. Use spaces between words unless the repo’s existing plans use a different convention.

## Required Plan Shape

Every plan must start with frontmatter:

```yaml
---
date: YYYY-MM-DD
status: in-progress
subject: short-subject
---
```

Preferred sections:

- `Goal`
- `Context`
- `Decisions`
- `Implementation Steps`
- `Learning Log`
- `Work Log`
- `Unfinished Work`

Use `Learning Log` for durable design decisions, architecture notes, debugging findings, constraints, and tradeoffs that future sessions need.

Use `Work Log` for chronological execution notes. Every item must be a checkbox bullet with a local timestamp:

```markdown
- [x] 2026-06-12 14:11 - Checkpointed existing work before edits: `fecffcc`.
- [ ] 2026-06-12 14:42 - Run final browser verification after the CSS tweak.
```

Use `YYYY-MM-DD HH:MM` local time. Use checked items for completed work and unchecked items for planned or in-progress work.

Use `Unfinished Work` when any work remains. Keep it short and actionable, and remove or check items as they are resolved.

## Workflow

1. Inspect repo guidance and existing plans.
   - Read repo-local `AGENTS.md`.
   - List `agent-plans/`.
   - Identify whether to update the active plan or create the next numbered plan.

2. Create or update the plan before meaningful edits.
   - Capture the user request in `Goal` and `Context`.
   - Record decisions already made.
   - Add concrete implementation steps.
   - Add an initial checked `Work Log` item with the current local timestamp.
   - Add `Unfinished Work` checkboxes for the remaining work.

3. Keep the plan current while working.
   - Update decisions when scope changes.
   - Add learning-log notes after debugging or architectural choices.
   - Check off work-log items as they actually complete.
   - Add new unfinished work when the user adds scope.

4. Checkpoint in git frequently.
   - Use the repo’s required checkpoint mechanism.
   - If the repo requires `git checkpoint`, use that instead of bare `git commit`.
   - Do not checkpoint in a side conversation unless the user explicitly asks for it there.

5. Finish with verification and status.
   - Run relevant tests/build/browser checks for the actual task.
   - Mark plan status complete only when the implementation is done and verified.
   - Leave status in-progress when the plan intentionally records future work.

## Side Conversations

When operating in a side conversation, do only the requested lightweight planning mutation unless the user explicitly asks for implementation. Avoid source edits, git changes, long-running commands, or broad workspace changes.

## Good Plan Entries

Good:

```markdown
- [x] 2026-06-12 17:03 - Created the scan-excludes plan from the requested task.
- [ ] 2026-06-12 17:10 - Verify update scans respect per-scan excludes.
```

Bad:

```markdown
- did stuff
- TODO later
```

## Completion Criteria

A plan update is complete when:

- The correct plan file exists or was updated.
- Frontmatter is valid.
- Decisions and current scope are captured.
- Work Log uses checkbox bullets with `YYYY-MM-DD HH:MM` timestamps.
- Unfinished Work reflects what remains.
- Any requested checkpoint behavior has followed repo rules.
