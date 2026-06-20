# agents-anthropic-claude

Claude Code material for Gymnasium. The goal is to collect guidance, plugins, and
skills tuned for Claude agents — to reduce papercuts and stabilize agents so they
can run longer tasks without breaking down.

The top level is a general home for Claude-specific material; the plugin
marketplace lives in its own `collection/` subfolder so it does not own the whole
directory.

## Layout

```text
agents-anthropic-claude/
  CLAUDE.md                            Curated, in-progress agent guidance
  collection/                          Native Claude Code plugin marketplace
    .claude-plugin/marketplace.json    Marketplace manifest
    plugins/                           Plugin sources (pluginRoot)
      <plugin>/.claude-plugin/plugin.json
      <plugin>/{skills,agents,commands,hooks}/
```

`collection/` is the marketplace root (the directory containing
`.claude-plugin/`). Plugin `source` paths in `marketplace.json` resolve relative
to it, and `metadata.pluginRoot` is set to `./plugins` so entries can use short
sources like `"source": "my-plugin"`.

## Install

Add the marketplace from a local checkout:

```sh
/plugin marketplace add /Users/nonlogical/Projects/local/gymnasium/agents-anthropic-claude/collection
/plugin install <plugin>@gymnasium-claude
```

Once the repo is hosted, users can add it by git URL with a `git-subdir` source
pointing at `agents-anthropic-claude/collection`.

The marketplace name is `gymnasium-claude` (distinct from the Codex catalog's
`gymnasium`, since each user registers one marketplace per name).

## Contents

- `CLAUDE.md` — curated guidance for stabilizing long-running Claude agents.
- `collection/.claude-plugin/marketplace.json` — Claude Code marketplace manifest
  (empty plugin list for now).
- `collection/plugins/` — placeholder for future Claude plugins.

Shared, runtime-agnostic skills live in the repo's top-level `agents/skills/`.
