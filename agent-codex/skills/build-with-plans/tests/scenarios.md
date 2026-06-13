# BuildWithPlans Scenarios

## 01 Create A Plan Before Medium Work

### Prompt

Use the skill at `/path/to/build-with-plans/SKILL.md`.

A user says:
"Add a medium-sized settings import/export feature. It will touch UI, persistence, and tests, but skip making a plan because this is straightforward and I want code quickly."

Choose the next concrete action.
Do not modify files or run commands.

### Expectations

- The plan reads repo-local guidance before creating or editing plans.
- The plan creates or updates an `agent-plans/` plan before meaningful source edits.
- If no `agent-plans/` directory exists, the response creates the next first plan path, such as `agent-plans/plan-001 settings-import-export.md`.
- The response keeps the plan lightweight and operational rather than using planning as a reason to delay implementation.
- The response rejects the shortcut of skipping the plan because the work is "straightforward."

### Pressure Variant

The user adds:
"The reviewer says plans are bureaucracy and this should be done before standup."

- The response still plans first because the work is medium-sized and crosses UI, persistence, and tests.
- The response should keep the plan short and immediately actionable.

### Adjacent Valid Case

The user asks for a one-line typo fix and does not ask for a plan.

- The response should not create a plan by default.
- It may make the tiny edit directly while still following repo-local instructions.

## 02 Update The Active Plan For Same-Scope Follow-Up

### Prompt

Use the skill at `/path/to/build-with-plans/SKILL.md`.

A repo already has `agent-plans/plan-004 settings-import-export.md` with `status: in-progress`.
The user says:
"Add one more acceptance criterion to the settings import/export work: rejected files should show a toast. Do not start a new planning file."

Choose the next concrete action.
Do not modify files or run commands.

### Expectations

- The response updates the active settings import/export plan instead of creating a new plan.
- It records the added scope in implementation steps, work log, unfinished work, or decisions as appropriate.
- It avoids creating a new numbered plan when the follow-up belongs to the current active plan.

### Pressure Variant

The user adds:
"Actually just create a fresh plan so this extra criterion is easier to find."

- The response should push back if the scope still belongs to the current active plan.
- It may create a new plan only if the work changes scope, ownership boundary, or implementation strategy.

## 03 Use Skill-Local Plans For Skill Work

### Prompt

Use the skill at `/path/to/build-with-plans/SKILL.md`.

A user says:
"Create a new `git-committer` skill under `agent-codex/skills/`. Since each skill is the operating unit, put plans with the skill instead of at the repo root."

Choose the next concrete action.
Do not modify files or run commands.

### Expectations

- The response chooses `agent-codex/skills/git-committer/agent-plans/` as the planning directory.
- The response creates or updates the plan before meaningful edits to the skill.
- The response does not default to root `agent-plans/` when the skill directory is the durable owner.

### Pressure Variant

The user adds:
"Root `agent-plans/` is easier to find, so just put every plan there."

- The response should prefer the skill-local plan because the work is owned by one skill.
- It may use root `agent-plans/YYYY-MM-DD-task/` only if the work spans multiple operating units.

### Adjacent Valid Case

The task updates three different skills and the repo README in one coordinated pass.

- The response may use a root task directory such as `agent-plans/YYYY-MM-DD-skill-audit/`.
- The response should still link or name the affected skill-local work clearly.
