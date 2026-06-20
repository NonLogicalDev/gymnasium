---
name: Search_QMD
description: "Search the vault using QMD semantic search. Use proactively for past decisions, people, notes, or vault content. If QMD is sandbox-blocked, fix the wrapper or request approval; do not silently bypass it."
---

# Search QMD — Vault Semantic Search

Use `$Search_QMD` in chat. The skill name is `Search_QMD`.

Before reading vault files directly, search with QMD first. It returns relevant snippets without burning context on full file reads.

## Sandbox Behavior

Always invoke QMD through the Search_QMD helper. From a vault that links Gymnasium skills into `.agents/skills`, use `.agents/skills/Search_QMD/scripts/qmd`. In this repo, the canonical helper lives at `agents/skills/Search_QMD/scripts/qmd`.

The wrapper pins QMD's mutable config/cache state under the target vault's `.qmd-agent/` directory so sandboxed agents can read and write the SQLite index without falling back to direct file search. It resolves the target vault in this order:

1. `QMD_VAULT_ROOT`
2. The current git worktree root
3. The current working directory

If QMD fails with `sqlite-vec extension is unavailable`, `unable to open database file`, or another sandbox-looking storage error, treat that as a wrapper/setup regression. Fix `agents/skills/Search_QMD/scripts/qmd` or request approval to run the same wrapper unsandboxed. Do not silently fall back to `rg`, `find`, or direct note reads for vault discovery unless the user explicitly tells you to bypass QMD.

## Invocation

All qmd commands go through the wrapper at `.agents/skills/Search_QMD/scripts/qmd` when run from a linked vault. The wrapper uses `bunx` if available and falls back to `nix run nixpkgs#bun -- x @tobilu/qmd`. Use the wrapper path in any scripts or hooks; in interactive terminal use you can call it directly or alias it.

```bash
.agents/skills/Search_QMD/scripts/qmd query "..."
```

If you are invoking the helper from outside the vault, set `QMD_VAULT_ROOT` explicitly:

```bash
QMD_VAULT_ROOT=/path/to/vault agents/skills/Search_QMD/scripts/qmd query "..."
```

To make `qmd` available as a bare command in your shell, add bun to your nix profile:
```bash
nix profile install nixpkgs#bun
# then: bunx @tobilu/qmd ...
# or alias qmd='bunx @tobilu/qmd' in your shell config
```

## Commands

### Search (pick one per query)
- `.agents/skills/Search_QMD/scripts/qmd query "..." --json -n 10` — Best quality. Hybrid BM25 + vector + LLM reranking. Use for complex or conceptual queries.
- `.agents/skills/Search_QMD/scripts/qmd search "..." --json -n 10` — Fast BM25 keyword. Use for exact terms, names, dates.
- `.agents/skills/Search_QMD/scripts/qmd vsearch "..." --json -n 5` — Semantic only. Use for exploratory queries where you don't know the exact words.

### Retrieve
- `.agents/skills/Search_QMD/scripts/qmd get "path/to/file.md"` — Full document by path.
- `.agents/skills/Search_QMD/scripts/qmd get "#docid"` — Full document by ID (from search results).
- `.agents/skills/Search_QMD/scripts/qmd multi-get "Pages/*.md" --json -l 40` — Batch retrieve by glob pattern.

### Index Management
- `.agents/skills/Search_QMD/scripts/qmd update` — Re-index after file changes (fast, ~1–2s incremental). Run when lookup needs a fresh index; do not run it automatically at every session start.
- `.agents/skills/Search_QMD/scripts/qmd embed` — Regenerate vector embeddings. Run after bulk note creation.

## When to Search

| Situation | Command |
|-----------|---------|
| User asks about a past decision | `.agents/skills/Search_QMD/scripts/qmd query "decision about <topic>"` |
| User mentions a person by name | `.agents/skills/Search_QMD/scripts/qmd search "<name>"` |
| Before creating a new note | `.agents/skills/Search_QMD/scripts/qmd vsearch "<topic>"` — check for existing content first |
| After creating a note | `.agents/skills/Search_QMD/scripts/qmd vsearch "<note title>"` — find notes that should link to it |
| User asks about a topic in the vault | `.agents/skills/Search_QMD/scripts/qmd query "<topic>"` |
| Loading context on a company | `.agents/skills/Search_QMD/scripts/qmd search "<company name>"` |

## Vault Collections

This vault is registered as a single collection:

- **Collection:** `vault`
- **Mask:** `**/*.md`
- **URI:** `qmd://vault`

## After Bulk Changes

Run `.agents/skills/Search_QMD/scripts/qmd update && .agents/skills/Search_QMD/scripts/qmd embed` to keep the index fresh after bulk note changes. Do not run maintenance at every session start unless the task needs a fresh local lookup.

## Setup (first time only)

```bash
.agents/skills/Search_QMD/scripts/qmd collection add . --name vault --mask "**/*.md"
.agents/skills/Search_QMD/scripts/qmd context add qmd://vault "Personal knowledge base: research notes, career, daily journals, web clippings"
.agents/skills/Search_QMD/scripts/qmd update && .agents/skills/Search_QMD/scripts/qmd embed
```

qmd is not yet in nixpkgs directly. The wrapper pulls it via `bunx` (bun's package runner). Bun is available in nixpkgs:
```bash
nix profile install nixpkgs#bun   # add bun permanently
# or use the wrapper which calls nix run nixpkgs#bun on-demand
```
