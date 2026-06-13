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
