# Gymnasium

Gymnasium is a local curation repo for agent skills and plugins.

The initial focus is Codex, with room to adapt useful patterns to other agent runtimes later.

## Layout

```text
agent-codex/
  .agents/plugins/marketplace.json
  skills/
    build-with-plans/
    delegator/
  plugins/
```

## Current Contents

- `agent-codex/.agents/plugins/marketplace.json` - Codex marketplace metadata for the Gymnasium plugin catalog.
- `agent-codex/skills/build-with-plans/` - local copy of the Build With Plans skill.
- `agent-codex/skills/delegator/` - skill for keeping the main thread lightweight while batching work through subagents.
- `agent-codex/skills/git-checkpoints/` - skill for creating frequent reversible git checkpoint commits.
- `agent-codex/skills/git-committer/` - skill for drafting crisp public-facing commit messages.
- `agent-codex/skills/writing-and-updating-skills/` - imported from `abhinav/home`; guidance for authoring and pressure-testing skills.
- `agent-codex/plugins/` - placeholder for future Codex plugins.
