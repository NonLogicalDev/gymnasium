---
name: Agent_Brain
description: Use when Codex needs to retrieve or store cross-project personal context in `~/brain`, including prior decisions, memories, daily logs, agent handoffs, current state, or durable user-facing context shared across repos and agent runtimes.
---

# Agent Brain

Treat `~/brain` as the user's central context system. Use it before scattering memory, handoff notes, or project history into the repo you happen to be working in.

## Core Contract

Resolve the brain root first. Default to `~/brain`; allow `AGENT_BRAIN_ROOT` to override it when the environment provides one. Verify the root exists before reading or writing.

Read `~/brain/AGENTS.md` before changing anything in the brain. Its rules for note format, folder choice, links, frontmatter, Obsidian moves, and git checkpointing own the final write behavior.

Use `$Search_QMD` for broad retrieval before direct file reads. Query the brain root, then read only the source files needed to answer or write accurately.

When reporting or persisting paths, use `~/brain/...` and other `~` conventions for user-home paths. Absolute user-directory paths are for immediate tool execution only; do not write them into repo guidance, plans, tests, or notes.

Do not store cross-project memory in a random project repo. If the content is meant to be remembered by future agents or shared across projects, route it into the brain.

Do not write raw chain-of-thought, secret values, credentials, private contact/payment identifiers, or bulky transcript dumps. Store concise facts, decisions, context, links, and next actions.

## Retrieval Workflow

1. Resolve the root:
   ```bash
   BRAIN_ROOT="${AGENT_BRAIN_ROOT:-$HOME/brain}"
   ```
2. Read `"$BRAIN_ROOT/AGENTS.md"` for current vault rules.
3. For active context, read `Brain/Northstar.md` and `Brain/Context.md`; read today's `Journals/YYYY-MM-DD.md` only when the current day may matter.
4. For broad lookup, use the Search_QMD helper with `--vault-root "$BRAIN_ROOT"`:
   ```bash
   ~/Projects/local/gymnasium/agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root "$BRAIN_ROOT" exec query "decision about <topic>" --json -n 10
   ```
5. Read the minimum exact files needed after QMD returns paths.
6. Return a retrieval packet: answer, source file paths, confidence, and any stale/missing context.

## Storage Surfaces

| Need | Primary location | Rule |
| --- | --- | --- |
| Daily log or raw user-facing session note | `Journals/YYYY-MM-DD.md` | Append only. Create the file with journal frontmatter if missing. |
| Current situational state for future agents | `Brain/Context.md` | Update only when explicitly asked or when preserving active state is the task. |
| Durable personal memory or significant event | `Brain/Memories.md` plus linked detail note when useful | Keep the index concise; create detail notes in `Pages/` or the relevant project folder. |
| Decision with project scope | `Projects/<category>/<project>/Decisions.md` plus one-line link in `Brain/Decisions.md` | Keep detailed rationale with the project; keep the brain index short. |
| Cross-project/global decision | `Brain/Decisions.md` and, when needed, a linked `Pages/` note | Make it discoverable without binding it to one repo. |
| General reusable reference | `Pages/` | Include frontmatter, links, and appropriate note-type/domain tags. |

## Write Workflow

1. Read `~/brain/AGENTS.md` before planning or making edits, and include that read as the first step of any edit plan.
2. Confirm the user asked to store or update brain content, or that the task explicitly requires a durable handoff.
3. Choose the storage surface from the table before editing.
4. Search first for existing notes to avoid duplicates.
5. Follow vault frontmatter, tag, wikilink, filename, and markdown indentation rules from `AGENTS.md`.
6. Preserve user voice. Prefer concise bullets and source links over polished prose.
7. Report storage surfaces as `~/brain/...` paths unless a tool specifically requires the resolved absolute path.
8. After meaningful brain edits, follow the brain repo checkpoint rule unless the user says not to.

## Agent Handoffs

For handoffs to future agents, write a compact operational note, not a transcript:

- What changed or was decided.
- Where the source files or notes live.
- Current blockers and next actions.
- Validation already run.
- Date and relevant project links.

Use today's journal for ephemeral handoff notes. Use `Brain/Context.md` only for state that future sessions should load at startup.

## Conflict Rules

- If repo-local instructions and brain instructions conflict while writing brain files, brain `AGENTS.md` wins for those files.
- If a project has its own `Projects/...` folder in the brain, store project decisions there instead of in a repo README.
- If the user asks for "remember this" but the content is sensitive, summarize the durable non-secret fact and omit the sensitive value.
- If the right storage surface is unclear, ask one short clarifying question instead of guessing.

## Tests

When changing this skill, read [tests/README.md](tests/README.md). Run the relevant scenarios with fresh subagents that have empty context windows.
