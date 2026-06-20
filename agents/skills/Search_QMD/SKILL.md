---
name: Search_QMD
description: "Search the vault using QMD semantic search. Use proactively for past decisions, people, notes, or vault content. If QMD is sandbox-blocked, fix the wrapper or request approval; do not silently bypass it."
---

# Search QMD — Vault Semantic Search

Use `$Search_QMD` in chat. The skill name is `Search_QMD`.

Before reading vault files directly, search with QMD first. It returns relevant snippets without burning context on full file reads.

## Sandbox Behavior

Always invoke QMD through the Search_QMD helper. From the Gymnasium repo root, use `agents/skills/Search_QMD/scripts/qmd-vault-helper`. From inside this skill folder, use `scripts/qmd-vault-helper`. Do not assume a vault-local `.agents` installation path.

The helper pins QMD's mutable config/cache state under the target vault's `.qmd-agent/` directory so sandboxed agents can read and write the SQLite index without falling back to direct file search. Pass `--vault-root` or set `QMD_VAULT_ROOT`; one of them is required and must point at the vault root.

If QMD fails with `sqlite-vec extension is unavailable`, `unable to open database file`, or another sandbox-looking storage error, treat that as a helper/setup regression. Fix `agents/skills/Search_QMD/scripts/qmd-vault-helper` or request approval to run the same helper unsandboxed. Do not silently fall back to `rg`, `find`, or direct note reads for vault discovery unless the user explicitly tells you to bypass QMD.

## Invocation

All qmd commands go through `qmd-vault-helper`. The helper is standalone inside this skill; resolve it relative to this `SKILL.md` or call it from the Gymnasium repo root.

Agents can inspect the helper contract without environment setup:

```bash
agents/skills/Search_QMD/scripts/qmd-vault-helper --help
```

```bash
agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault -- query "..."
```

From inside the Search_QMD skill folder:

```bash
scripts/qmd-vault-helper --vault-root /path/to/vault -- query "..."
```

`QMD_INVOKE_METHOD` is optional. Set it to one method or a comma-separated ordered list. The default is `local,bun-bundle,bunx,nix`.

| Method | Behavior |
|--------|----------|
| `local` | Execute `qmd` from `PATH`. Override the binary name with `QMD_LOCAL_BINARY`. |
| `bun-bundle` | On first use, compile the QMD package with `bun build --compile` into the helper's `XDG_CACHE_HOME` and reuse the packed executable on later runs. |
| `bunx` | Run QMD through `bunx`, or `bun x` when `bunx` is absent. |
| `nix` | Execute QMD through `nix run nixpkgs#bun -- x @tobilu/qmd`. |

Examples:

```bash
agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault --invoke-method local -- query "..."
agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault --invoke-method bun-bundle -- query "..."
agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault --invoke-method bunx -- query "..."
agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault --invoke-method local,bun-bundle,bunx,nix -- query "..."
```

## Commands

### Search (pick one per query)
- `agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault -- query "..." --json -n 10` — Best quality. Hybrid BM25 + vector + LLM reranking. Use for complex or conceptual queries.
- `agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault -- search "..." --json -n 10` — Fast BM25 keyword. Use for exact terms, names, dates.
- `agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault -- vsearch "..." --json -n 5` — Semantic only. Use for exploratory queries where you don't know the exact words.

### Retrieve
- `agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault -- get "path/to/file.md"` — Full document by path.
- `agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault -- get "#docid"` — Full document by ID (from search results).
- `agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault -- multi-get "Pages/*.md" --json -l 40` — Batch retrieve by glob pattern.

### Index Management
- `agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault -- update` — Re-index after file changes (fast, ~1–2s incremental). Run when lookup needs a fresh index; do not run it automatically at every session start.
- `agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault -- embed` — Regenerate vector embeddings. Run after bulk note creation.

## When to Search

| Situation | Command |
|-----------|---------|
| User asks about a past decision | `agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault -- query "decision about <topic>"` |
| User mentions a person by name | `agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault -- search "<name>"` |
| Before creating a new note | `agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault -- vsearch "<topic>"` — check for existing content first |
| After creating a note | `agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault -- vsearch "<note title>"` — find notes that should link to it |
| User asks about a topic in the vault | `agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault -- query "<topic>"` |
| Loading context on a company | `agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault -- search "<company name>"` |

## Vault Collections

This vault is registered as a single collection:

- **Collection:** `vault`
- **Mask:** `**/*.md`
- **URI:** `qmd://vault`

## After Bulk Changes

Run `agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault -- update && agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault -- embed` to keep the index fresh after bulk note changes. Do not run maintenance at every session start unless the task needs a fresh local lookup.

## Setup (first time only)

```bash
agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault -- collection add . --name vault --mask "**/*.md"
agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault -- context add qmd://vault "Personal knowledge base: research notes, career, daily journals, web clippings"
agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault -- update && agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault -- embed
```

qmd is not yet in nixpkgs directly. The `nix` invocation method runs QMD through Bun from nixpkgs:
```bash
agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root /path/to/vault --invoke-method nix -- query "..."
```
