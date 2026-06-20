---
name: Agent_Brain
description: Use when Codex needs to retrieve or store cross-project personal context through the configured brain root, including prior decisions, memories, daily logs, agent handoffs, current state, or durable user-facing context shared across repos and agent runtimes.
---

# Agent Brain

Treat the resolved brain root as the user's central context system. Use it before scattering memory, handoff notes, or project history into the repo you happen to be working in.

## Core Contract

Resolve `<brain-root>` first. Use `AGENT_BRAIN_ROOT` when the environment provides one; otherwise use the brain root configured by active global or repo agent instructions. If no root can be determined, ask one short clarifying question instead of assuming a literal path. Other skills should invoke `$Agent_Brain` for this decision instead of hard-coding a brain path.

Verify the brain root is an Obsidian-style markdown vault before applying vault folder assumptions. Prefer a clear `.obsidian/` directory; also look for vault markers such as `AGENTS.md`, `Brain/`, `Journals/`, `Pages/`, `Projects/`, and markdown notes. If the root does not look like an Obsidian-style vault, ask before reading broadly or writing.

Read `<brain-root>/AGENTS.md` before changing anything in the brain. Its rules for note format, folder choice, links, frontmatter, Obsidian moves, git checkpointing, and user-specific organizational preferences or deviations own the final write behavior. If `AGENTS.md` conflicts with this generic skill, `AGENTS.md` wins. If the root otherwise looks like an Obsidian-style vault but `AGENTS.md` is missing, retrieval may proceed read-only with a missing-guidance caveat, but writes must pause for confirmation before creating or changing files.

Use `$Search_QMD` for broad retrieval before direct file reads. Query the brain root, then read only the source files needed to answer or write accurately.

When reporting or persisting paths, use `<brain-root>/...` or the user-facing shorthand chosen by `$Agent_Brain`. Absolute user-directory paths are for immediate tool execution only; do not write them into repo guidance, plans, tests, or notes.

Do not store cross-project memory in a random project repo. If the content is meant to be remembered by future agents or shared across projects, route it into the brain.

Do not write raw chain-of-thought, secret values, credentials, private contact/payment identifiers, or bulky transcript dumps. Store concise facts, decisions, context, links, and next actions.

## Retrieval Workflow

1. Resolve `<brain-root>` from `AGENT_BRAIN_ROOT`, active agent instructions, or explicit user context. Set `BRAIN_ROOT` to that resolved path for commands.
2. Verify `"$BRAIN_ROOT"` exists and looks like an Obsidian-style markdown vault. Prefer `.obsidian/`; otherwise require enough vault markers to avoid treating a random directory as the brain.
3. Read `"$BRAIN_ROOT/AGENTS.md"` for current vault rules, including user preferences or deviations about organization. If it is missing, report that caveat and stay read-only unless the user confirms how to proceed.
4. For active context, read `Brain/Northstar.md` and `Brain/Context.md`; read today's `Journals/YYYY-MM-DD.md` only when the current day may matter.
5. For broad lookup, use the Search_QMD helper with `--vault-root "$BRAIN_ROOT"`:
   ```bash
   ~/Projects/local/gymnasium/agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root "$BRAIN_ROOT" exec query "decision about <topic>" --json -n 10
   ```
6. Read the minimum exact files needed after QMD returns paths.
7. Return a retrieval packet: answer, source file paths, confidence, and any stale/missing context.

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

1. Resolve `<brain-root>`, verify it is an Obsidian-style markdown vault, then read `<brain-root>/AGENTS.md` before planning or making edits. Include these checks as the first steps of any edit plan. If `AGENTS.md` is missing, ask before writing.
2. Apply any organization preferences or deviations from `AGENTS.md` before choosing a storage surface.
3. Confirm the user asked to store or update brain content, or that the task explicitly requires a durable handoff.
4. Choose the storage surface from the table before editing.
5. Search first for existing notes to avoid duplicates.
6. Follow vault frontmatter, tag, wikilink, filename, and markdown indentation rules from `AGENTS.md`.
7. Preserve user voice. Prefer concise bullets and source links over polished prose.
8. Report storage surfaces as `<brain-root>/...` or the configured user-facing shorthand unless a tool specifically requires the resolved absolute path.
9. After meaningful brain edits, follow the brain repo checkpoint rule unless the user says not to.

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
