---
name: Tasker_Plan
description: Maintain numbered `agents-plans` while building. Use when the user invokes $Tasker_Plan, asks to add a task or plan, starts medium-sized design or functionality work, or needs durable decisions, product-integration notes, dated project or epic slugs, timestamped work logs, unfinished work, and frequent git checkpoints.
---

# Tasker Plan

Use this skill to keep implementation work durable across sessions. It turns medium-sized design or functionality changes into numbered plan files that capture decisions, learnings, work logs, and unfinished work while the code evolves.

The user-facing invocation is `$Tasker_Plan`.
The skill name is `Tasker_Plan`,
and metadata prompts should refer to it as `$Tasker_Plan`.

## Core Rules

- Read the repo-local `AGENTS.md` before creating or editing plans. Repo instructions override this skill when they are more specific.
- Put every plan in one of the two allowed `agents-plans` layouts in this skill. Do not invent ad hoc plan locations.
- Create or update a plan for every medium-sized design or functionality change.
- Do not create a new plan for tiny one-off edits unless the user asks.
- When a follow-up request belongs to the current active plan, update that existing plan instead of creating a new one.
- Create a new plan when the work changes scope, ownership boundary, or implementation strategy.
- Keep plans concise but operational: future sessions should understand what was decided, what changed, and what remains.
- Treat non-trivial product, UI, and architecture requests as integration work, not tickets to attach to the nearest existing surface.
- Prefer a clean product model over the smallest local patch when the current model is showing strain. If the clean model implies moving, merging, deleting, or re-architecting existing pieces, capture that explicitly before implementation.
- Checkpoint often in git according to repo rules, especially before risky edits, after verified milestones, before switching context, and before ending a session with meaningful changes.

## Plan Location

Choose exactly one of these planning locations:

```text
~/brain/Projects/<category>/<PRJ_SLUG>/<EPIC_SLUG>/agents-plans/<PLAN_SLUG>.md
<REPO_ROOT>/.agents-plans/<EPIC_SLUG>/<PLAN_SLUG>.md
```

Use the brain-project layout when the work belongs to a tracked project or task epic in the brain, especially when the plan should survive across repos, checkouts, or implementation surfaces.

Use the repo-root layout when the work is owned by one repository checkout and should live with that repo.

If an existing active plan clearly owns the follow-up, update it even if you would otherwise choose the other allowed layout.

Do not use skill-local `.agent-plans/`, repo-root `.agent-plans/`, or ad hoc task directories unless repo-local instructions explicitly override this skill.

## Naming Contract

Use these slug shapes:

```text
<PRJ_SLUG> = YYYY-MM-DD <PRJ_SUBJECT_SLUG>
<EPIC_SLUG> = epic-<N> YYYY-MM-DD <EPIC_SUBJECT_SLUG>
<PLAN_SLUG> = plan-<N> YYYY-MM-DD <PLAN_SUBJECT_SLUG>
```

`<PRJ_SUBJECT_SLUG>`, `<EPIC_SUBJECT_SLUG>`, and `<PLAN_SUBJECT_SLUG>` must use lowercase letters, digits, and `-` only. No underscores. No dots. No spaces inside the subject slug.

Increment `<N>` within the parent container:

- Project slug dates identify when the project container was created.
- `epic-<N>` increments within the project or repo root.
- In brain-project layout, `plan-<N>` increments within the epic's `agents-plans` directory.
- In repo-root layout, `plan-<N>` increments within `.agents-plans/<EPIC_SLUG>/`.

Examples:

```text
2026-06-18 personal-icloud-backup
epic-2 2026-06-18 auth-refresh
plan-1 2026-06-18 retry-and-timeouts
```

Use the slug as the plan document basename. In markdown repos, that normally means `<PLAN_SLUG>.md`.

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
   - Decide whether the work belongs in the brain-project layout or the repo-root layout.
   - If using the brain-project layout, identify `<category>`, `<PRJ_SLUG>`, and `<EPIC_SLUG>`.
   - If using the repo-root layout, identify `<EPIC_SLUG>`.
   - List existing plans in the chosen epic plan directory: `agents-plans/` for brain-project layout or `.agents-plans/<EPIC_SLUG>/` for repo-root layout.
   - Identify whether to update the active plan or create the next numbered `plan-<N>` slug.

2. Create or update the plan before meaningful edits.
   - Capture the user request in `Goal` and `Context`.
   - Add `Product Integration` for medium or larger product, UI, or architecture changes.
   - Use the integration pass to decide whether existing product or architecture pieces should move, merge, disappear, or be reworked instead of preserving the current shape.
   - Record decisions already made.
   - Add concrete implementation steps.
   - Add an initial checked `Work Log` item with the current local timestamp.
   - Add `Unfinished Work` checkboxes for the remaining work.
   - If creating a new plan, place it at the chosen path using the required slug schema.

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
- The plan lives in one of the two allowed `agents-plans` layouts.
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
