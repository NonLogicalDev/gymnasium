# Tasker Plan Scenarios

## 01 Use Repo-Root Epic Plans For Repo-Owned Work

### Prompt

Use the skill at `/path/to/Tasker_Plan/SKILL.md`.

A user says:
"Add a medium-sized settings import/export feature in this repo. It will touch UI, persistence, and tests. Keep the plan with the repo."

Choose the next concrete action.
Do not modify files or run commands.

### Expectations

- The response creates or updates a plan before meaningful source edits.
- The response chooses the repo-root layout: `<REPO_ROOT>/.agents-plans/<EPIC_SLUG>/<PLAN_SLUG>`.
- The response does not choose a skill-local `.agent-plans/` directory.
- If no epic exists yet, the response creates a dated epic slug such as `epic-1 2026-06-18 settings-import-export`.
- If no plan exists yet, the response creates a dated plan slug such as `plan-1 2026-06-18 import-export-flow`.
- The response keeps the plan lightweight and operational rather than using planning as a reason to delay implementation.

### Pressure Variant

The user adds:
"This is straightforward. Just drop `plan-001` under `.agent-plans/` so we can start coding."

- The response still uses the repo-root `.agents-plans/<EPIC_SLUG>/<PLAN_SLUG>` layout.
- The response rejects the shortcut of using the old `.agent-plans/` location and `plan-001` naming.

## 02 Update The Active Repo-Root Plan

### Prompt

Use the skill at `/path/to/Tasker_Plan/SKILL.md`.

A repo already has:
`.agents-plans/epic-2 2026-06-18 settings-import-export/plan-3 2026-06-18 import-validation.md`
with `status: in-progress`.

The user says:
"Add one more acceptance criterion to the current settings import/export work: rejected files should show a toast. Do not start a new planning file."

Choose the next concrete action.
Do not modify files or run commands.

### Expectations

- The response updates the existing in-progress plan instead of creating a new plan.
- It records the added scope in implementation steps, work log, unfinished work, or decisions as appropriate.
- It keeps the work under the existing repo-root epic path.
- It avoids creating a new numbered plan when the follow-up belongs to the active plan.

### Pressure Variant

The user adds:
"Actually just create `plan-4` so the extra criterion is easier to find."

- The response should push back if the scope still belongs to the current active plan.
- It may create a new plan only if the work changes scope, ownership boundary, or implementation strategy.

## 03 Use Brain Project Epic Plans For Project Work

### Prompt

Use the skill at `/path/to/Tasker_Plan/SKILL.md`.

A user says:
"This work belongs to the brain project, not just one repo. Put the plan under `~/brain/Projects/ongoing/` for the `personal-icloud-backup` project and create an auth-refresh epic."

Choose the next concrete action.
Do not modify files or run commands.

### Expectations

- The response chooses the brain-project layout:
  `~/brain/Projects/<category>/<PRJ_SLUG>/<EPIC_SLUG>/agents-plans/<PLAN_SLUG>`.
- The response uses a project slug like `2026-06-18 personal-icloud-backup`.
- The response uses an epic slug like `epic-1 2026-06-18 auth-refresh`.
- The response creates or updates a plan before meaningful edits.
- The response does not collapse the work back into repo-root `.agents-plans/` when the user explicitly wants project-owned planning.

### Adjacent Valid Case

The user says the work is confined to one checkout and should live with that repo.

- The response may choose the repo-root `.agents-plans/<EPIC_SLUG>/<PLAN_SLUG>` layout instead.

## 04 Enforce Slug Hygiene

### Prompt

Use the skill at `/path/to/Tasker_Plan/SKILL.md`.

A user says:
"Create a project `2026-06-18 Personal_Backup`, epic `epic-1 2026-06-18 oauth.v2`, and plan `plan-1 2026-06-18 Retry Logic`."

Choose the next concrete action.
Do not modify files or run commands.

### Expectations

- The response normalizes the subject-slug segments to lowercase alphanumerics and `-` only.
- The response rejects or rewrites underscores, dots, and spaces inside the subject slugs.
- The response keeps the dated `PRJ_SLUG`, `EPIC_SLUG`, and `PLAN_SLUG` structure intact.
- The response produces examples such as `2026-06-18 personal-backup`, `epic-1 2026-06-18 oauth-v2`, and `plan-1 2026-06-18 retry-logic`.

### Pressure Variant

The user adds:
"Keep my exact capitalization and punctuation because it reads better."

- The response still preserves the slug contract for the path.
- The response may keep a prettier human description inside the plan body, but not in the slug fields.

## 05 Product Integration Before Additive UI

### Prompt

Use the skill at `/path/to/Tasker_Plan/SKILL.md`.

A product already has one segmented control:
`Overview | Details`.

The user says:
"Add a Compare tab inside Details so users can switch between the current detail table and a comparison tree. This should be quick."

Choose the next concrete action.
Do not modify files or run commands.

### Expectations

- The response creates or updates a plan before meaningful source edits because this is a medium user-facing product change.
- The plan includes a `Product Integration` section.
- The response uses one of the allowed `agents-plans` layouts rather than ad hoc planning storage.
- The response does not immediately accept the literal nested-tab shape.
- The response identifies the underlying requirement as adding a new product mode or workflow, not merely placing another tab under the nearest existing surface.
- The response considers whether the cleaner model is one peer-level mode selector such as `Overview | Details | Compare`, a renamed selector, or another restructured model.
- The response explicitly records which existing pieces may need to move, merge, disappear, or be re-architected.

### Pressure Variant

The user adds:
"Don't overthink it. Just put Compare under Details and skip the plan shape discussion."

- The response still runs the product integration pass before implementation.
- The response should not silently preserve the old structure just because it already works.
