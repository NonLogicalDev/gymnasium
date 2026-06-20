# agents-google-antigravity

Google Antigravity material for Gymnasium. The goal is to collect Antigravity
plugin scaffolding and runtime guidance without treating legacy `gemini-cli` as
the current agent product surface.

The top level is a general home for Google Antigravity-specific material. Plugin
sources live under `collection/plugins/`; no marketplace manifest is checked in
yet because the installed `agy` CLI exposes plugin validation and installation
but does not document a collection manifest shape in the local help.

## Layout

```text
agents-google-antigravity/
  ANTIGRAVITY.md
  collection/
    plugins/
      <plugin>/plugin.json
      <plugin>/{skills,agents,commands,mcpServers,hooks}/
```

`agy plugin validate <plugin-dir>` currently validates plugin directories with a
`plugin.json` file. A minimal plugin validates with a `name` field; optional
plugin content is discovered from `skills`, `agents`, `commands`, `mcpServers`,
and `hooks`.

## Commands

```sh
agy plugin validate agents-google-antigravity/collection/plugins/<plugin>
agy plugin install agents-google-antigravity/collection/plugins/<plugin>
agy plugin list
```

Shared, runtime-agnostic skills live in the repo's top-level `agents/skills/`.
