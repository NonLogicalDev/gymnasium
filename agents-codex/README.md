# agents-codex

Codex-specific material for Gymnasium. The top level is a general home for
Codex runtime assets; the plugin marketplace lives in `collection/` so it does
not own the whole directory.

## Layout

```text
agents-codex/
  README.md
  collection/                         Codex plugin marketplace root
    .agents/plugins/marketplace.json  Marketplace manifest
    plugins/                          Plugin sources
```

`collection/` is the Codex marketplace root. Plugin entries in
`marketplace.json` should use source paths relative to that directory.

## Contents

- `collection/.agents/plugins/marketplace.json` - Codex marketplace manifest
  for the `gymnasium` catalog.
- `collection/plugins/` - placeholder for future Codex plugins.

Shared, runtime-agnostic skills live in the repo's top-level `agents/skills/`.
