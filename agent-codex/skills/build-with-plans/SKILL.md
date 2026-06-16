---
name: build-with-plans
description: Maintain numbered .agent-plans while building. Use when the user invokes $BuildWithPlans, asks to add a task or plan, starts medium-sized design or functionality work, or works in a project with a .agent-plans directory and wants durable decisions, product-integration notes, timestamped work logs, unfinished work, and frequent git checkpoints.
---

# BuildWithPlans

Use this skill to keep implementation work durable across sessions. It turns medium-sized design or functionality changes into numbered plan files that capture decisions, learnings, work logs, and unfinished work while the code evolves.

The user-facing invocation is `$BuildWithPlans`.
The validated skill name is `build-with-plans`,
and metadata prompts should refer to it as `$build-with-plans`.

## Core Rules

- Read the repo-local `AGENTS.md` before creating or editing plans. Repo instructions override this skill when they are more specific.
- Put plans at the smallest durable operating unit unless the repo explicitly uses another location.
- Create or update a plan for every medium-sized design or functionality change.
- Do not create a new plan for tiny one-off edits unless the user asks.
- When a follow-up request belongs to the current active plan, update that existing plan instead of creating a new one.
- Create a new plan when the work changes scope, ownership boundary, or implementation strategy.
- Keep plans concise but operational: future sessions should understand what was decided, what changed, and what remains.
- Treat non-trivial product, UI, and architecture requests as integration work, not tickets to attach to the nearest existing surface.
- Prefer a clean product model over the smallest local patch when the current model is showing strain. If the clean model implies moving, merging, deleting, or re-architecting existing pieces, capture that explicitly before implementation.
- Checkpoint often in git according to repo rules, especially before risky edits, after verified milestones, before switching context, and before ending a session with meaningful changes.

## Plan Location

Choose the planning location by ownership boundary:

- Skill work: use `.agent-plans/` under that skill directory.
  Example: `agent-codex/skills/git-committer/.agent-plans/`.
- Plugin work: use `.agent-plans/` under that plugin directory.
- App, package, or service work: use `.agent-plans/` under that unit directory.
- Cross-cutting repo work: use a task directory under root `.agent-plans/`.
  Example: `.agent-plans/2026-06-12-settings-import/plan-001 import-flow.md`.
- If an existing active plan clearly owns the follow-up, update it even if another valid location exists.

Use the project root `.agent-plans/` only when no smaller durable unit owns the work or when repo-local instructions require it.

## Plan Naming

Use the next numeric plan id already present in the selected plan directory:

```text
.agent-plans/plan-NNN short-subject.md
```

Examples:

```text
.agent-plans/plan-007 table-polish.md
.agent-plans/plan-008 app-icon.md
.agent-plans/plan-009 scan-excludes.md
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
- `Product Integration`
- `Decisions`
- `Implementation Steps`
- `Learning Log`
- `Work Log`
- `Unfinished Work`

Use `Product Integration` for medium or larger product, UI, or architecture changes. Keep it short, but answer the integration questions directly:

- Existing product model
- New requirement's real intent
- Cleanest integrated model
- Existing pieces that should move, change, or disappear
- Architecture impact
- Why this is better than a local patch

If a task is purely mechanical or too small to need the section, omit it. If the work touches user-facing product shape, shared architecture, page structure, navigation, state model, or workflow composition, include it.

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
   - Identify the smallest durable operating unit for the work.
   - List existing plans in that unit's `.agent-plans/` directory, or in the cross-cutting root task directory when the work spans units.
   - Identify whether to update the active plan or create the next numbered plan.

2. Create or update the plan before meaningful edits.
   - Capture the user request in `Goal` and `Context`.
   - Add `Product Integration` for medium or larger product, UI, or architecture changes.
   - Use the integration pass to decide whether existing product or architecture pieces should move, merge, disappear, or be reworked instead of preserving the current shape.
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
   - For user-facing product changes, review whether the resulting system is more coherent, not merely whether the requested affordance exists.
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
- Product, UI, and architecture changes include an explicit integration note or a clear reason the section was not needed.
- Work Log uses checkbox bullets with `YYYY-MM-DD HH:MM` timestamps.
- Unfinished Work reflects what remains.
- Any requested checkpoint behavior has followed repo rules.

## Tests

When changing this skill,
read [tests/README.md](tests/README.md).
Run the relevant scenarios with fresh subagents that have empty context windows.
