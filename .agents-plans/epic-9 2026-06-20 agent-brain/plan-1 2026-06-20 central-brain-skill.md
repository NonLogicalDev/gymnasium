---
date: 2026-06-20
status: complete
subject: central-brain-skill
---

# Goal

Create `Agent_Brain`, a shared Gymnasium skill that makes the configured brain root the central place for agents to retrieve cross-project context and store user-facing memories, decisions, handoffs, and daily logs.

# Context

The brain root resolves to the personal vault through `$Agent_Brain`. The vault already has conventions for `Brain/`, `Journals/`, `Pages/`, and `Projects/`, plus QMD search through the Gymnasium `Search_QMD` helper. The new skill should make those conventions reusable by agents operating from arbitrary project repos.

# Product Integration

- Existing product model: Gymnasium owns reusable, runtime-agnostic skills under `agents/skills/`.
- New requirement's real intent: provide one agent-facing entrypoint for cross-project personal context and durable memory storage.
- Cleanest integrated model: create `agents/skills/Agent_Brain` with concise workflow guidance, UI metadata, and behavioral tests.
- Existing pieces that should move, change, or disappear: no existing skill owns the brain as an interface; `Search_QMD` remains the retrieval helper rather than being duplicated.
- Architecture impact: low; this adds a new skill that composes vault conventions and QMD guidance.
- Why this is better than a local patch: it prevents every project repo from inventing its own memory, handoff, or daily-log convention.

# Decisions

- Keep the folder and frontmatter name as `Agent_Brain` to match Gymnasium's existing shared skill naming convention.
- Route brain-root selection through `$Agent_Brain`; do not make other skills hard-code the brain directory.
- Use configured shorthand conventions in persisted repo docs, tests, and plans; do not write expanded user-home paths.
- Treat `Search_QMD` as the preferred retrieval mechanism for broad context lookup.
- Do not add helper scripts; this skill is a policy/workflow layer over existing vault and QMD tooling.
- Include persisted behavioral tests because the skill enforces discipline under pressure.

# Implementation Steps

1. Create `agents/skills/Agent_Brain/SKILL.md`.
2. Add `agents/skills/Agent_Brain/agents/openai.yaml`.
3. Add behavioral test scenarios for retrieval, memory storage, daily logs, and anti-scattering pressure.
4. Validate frontmatter, file layout, and scenario coverage.
5. Commit the scoped Gymnasium changes without touching unrelated edits.

# Work Log

- [x] 2026-06-20 15:37 - Read skill creation, hardening, planning, repo guidance, and brain-vault structure.
- [x] 2026-06-20 15:37 - Confirmed the configured brain root resolves to the personal vault.
- [x] 2026-06-20 15:37 - Drafted the `Agent_Brain` skill and tests.
- [x] 2026-06-20 15:43 - Ran the official skill validator through `uv`; it failed only because the validator enforces hyphen-case while Gymnasium uses names like `Agent_Brain`, `Search_QMD`, and `Tasker_Plan`.
- [x] 2026-06-20 15:46 - Forward-tested retrieval and sensitive-memory scenarios successfully with fresh subagents.
- [x] 2026-06-20 15:47 - Forward-tested daily-log storage; it chose the right journal surface but omitted the explicit `AGENTS.md` read step.
- [x] 2026-06-20 15:47 - Moved the brain `AGENTS.md` read requirement into the first Write Workflow step and reran the daily-log scenario.
- [x] 2026-06-20 15:49 - Added Codex papercut guidance and removed expanded user-home paths from the new Agent_Brain files plus nearby QMD plans/tests.
- [x] 2026-06-20 15:49 - Tightened Agent_Brain to require configured shorthand path reporting and to surface the `<brain-root>/AGENTS.md` read as the first edit-plan step.
- [x] 2026-06-20 15:50 - Reran the daily-log forward test; it passed with `<brain-root>/Journals/...` and an explicit first `<brain-root>/AGENTS.md` read step.
- [x] 2026-06-20 15:52 - Updated skills and tests so `$Agent_Brain` owns brain-root selection and other skills do not hard-code the brain directory.
- [x] 2026-06-20 15:52 - Added Codex Papercut 4 clarifying that "global AGENTS.md" usually means the user-level file at `~/.codex/AGENTS.md`.

# Unfinished Work

- [x] Create the skill files.
- [x] Validate the skill mechanically and behaviorally after papercut repairs.
- [x] Checkpoint the Gymnasium repo with scoped changes only.

# Validation

- Frontmatter sanity check passed for `agents/skills/Agent_Brain/SKILL.md`.
- `git diff --check` passed for the new skill and plan.
- Official `quick_validate.py` could run after fetching PyYAML through `uv`, but rejected the local `Agent_Brain` name because it enforces generic hyphen-case rather than Gymnasium's established underscore naming convention.
- Retrieval forward test passed: the agent resolved the brain root, planned to read brain rules/current context, used QMD against the brain root, and did not search only the current repo.
- Sensitive-memory forward test passed: the agent refused to store the raw token and only considered storing the non-secret preference.
- Daily-log forward test exposed two wording gaps: it omitted the explicit `AGENTS.md` read step and reported an expanded home path. Both triggered targeted repairs.
- Daily-log retest passed after the targeted repairs.
- Expanded home path scan passed across `agents-openai-codex`, `agents/skills`, and `.agents-plans`.
- Direct brain-root literal scan passed across `agents/skills` and `agents-openai-codex`.
