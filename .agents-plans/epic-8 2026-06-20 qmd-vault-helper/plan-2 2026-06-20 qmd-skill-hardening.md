---
date: 2026-06-20
status: complete
subject: qmd-skill-hardening
---

# Goal

Harden `agents/skills/Search_QMD` so future agents reliably use the standalone helper against the intended vault, avoid cwd/path mistakes, and preserve QMD as the default discovery path under pressure.

# Context

The QMD helper was converted to `qmd-vault-helper` with explicit `init` and `exec` commands. The skill now needs behavior-level hardening: agents should know how to pick the vault root, when to initialize, how to avoid stale vault-local paths, and what to do when QMD setup fails.

# Product Integration

- Existing product model: Search_QMD is a reusable Gymnasium skill that packages vault lookup guidance plus one helper script.
- New requirement's real intent: make the skill transfer cleanly to future agents, not just document the current helper syntax.
- Cleanest integrated model: keep `SKILL.md` concise for runtime behavior, add focused reusable scenarios under `tests/`, and avoid expanding the helper unless validation reveals a real helper bug.
- Existing pieces that should move, change, or disappear: setup examples that depend on cwd should become vault-root explicit; generic skill text should stop implying one specific vault when used standalone.
- Architecture impact: low; this is skill instruction and behavioral-test hardening around the existing helper boundary.
- Why this is better than a local patch: it protects the portable-helper contract with realistic tests, so future edits are checked against agent behavior rather than markdown appearance.

# Decisions

- Treat this as the second plan in the QMD helper epic because it hardens the skill that owns the helper.
- Use Skill_Harden's reference-skill testing shape: baseline current skill behavior, then patch, then pressure-test and persist scenarios.
- Keep tests read-only and confined to decision prompts; subagents should not edit files or run mutating commands.
- Preserve the helper's current command contract unless testing exposes a helper-level defect.

# Implementation Steps

1. Run baseline subagent scenarios against the current Search_QMD skill.
2. Capture observed failures and generalize the durable behavior boundary.
3. Patch the smallest Search_QMD guidance that prevents those failures.
4. Add reusable Search_QMD behavioral tests for the boundaries.
5. Run pressure/adjoining scenarios with fresh subagents.
6. Run mechanical validation and checkpoint the Gymnasium repo without touching unrelated changes.

# Learning Log

- Current likely risk: `collection add .` setup examples can index the agent's cwd instead of the target vault when the helper is invoked from Gymnasium or another repo.

# Work Log

- [x] 2026-06-20 15:31 - Created this hardening plan after reading Skill_Harden, Tasker_Plan, repo guidance, Search_QMD, and the Skill_Harden test references.
- [x] 2026-06-20 15:35 - Ran baseline subagent scenarios against current Search_QMD behavior.
- [x] 2026-06-20 15:35 - Captured the setup failure: a baseline agent followed the current docs and would run `collection add .` from the Gymnasium repo while targeting the vault.
- [x] 2026-06-20 15:35 - Captured the storage-error pass: a baseline agent resisted urgent direct `rg` fallback and chose helper repair or unsandboxed helper approval.
- [x] 2026-06-20 15:39 - Patched Search_QMD around the invariant that setup and search commands must target the chosen vault root, not incidental cwd.
- [x] 2026-06-20 15:39 - Added reusable Search_QMD behavioral tests for setup root discipline, storage-error fallback pressure, and ambiguous vault roots.
- [x] 2026-06-20 15:42 - Pressure-tested the edited skill with fresh subagents: setup now avoided `collection add .`, storage-error pressure preserved QMD, and ambiguous-root pressure asked for/identified the vault root instead of using cwd.
- [x] 2026-06-20 15:44 - Ran mechanical validation and prepared a scoped commit that excludes the unrelated `Code_Projects` working-tree edit.

# Unfinished Work

- [x] Capture baseline results and concrete repair targets.
- [x] Patch Search_QMD guidance.
- [x] Add reusable behavioral tests.
- [x] Run pressure validation with fresh subagents.
- [x] Run mechanical validation and checkpoint Gymnasium changes.

# Validation

- Baseline setup scenario failed before the repair: the agent would run `collection add .` from the Gymnasium checkout while targeting `/Users/nonlogical/Notes/Personal-OBS`.
- Baseline storage-error scenario already passed before the repair: the agent chose helper repair or unsandboxed helper approval instead of direct `rg`.
- After the repair, the setup pressure scenario passed: the agent used `init`, `exec update`, `exec embed`, and `exec query` with `--vault-root /Users/nonlogical/Notes/Personal-OBS`, and explicitly rejected `collection add .` from Gymnasium.
- After the repair, the storage-error pressure scenario passed under senior-reviewer/time pressure.
- After the repair, the ambiguous-root scenario passed: the agent refused `--vault-root "$PWD"` from an app repo and planned to identify or ask for the target vault root before running QMD.
- `git diff --check -- agents/skills/Search_QMD/SKILL.md agents/skills/Search_QMD/tests <plan>` passed.
- `qmd-vault-helper --help` still prints the helper contract.
- Static scans found no stale executable setup examples for `collection add .`, old separator syntax, vault-local helper paths, old default method order, or removed helper options.
