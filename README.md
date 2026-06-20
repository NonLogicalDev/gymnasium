# Gymnasium

Gymnasium is a local curation repo for agent skills and plugins.

The initial focus is Codex, with room to adapt useful patterns to other agent runtimes later.

## Layout

```text
agents-codex/
  .agents/plugins/marketplace.json
  skills/
    Code_Checkpoint/
    Code_Commit/
    Code_Develop/
    Code_Projects/
    Search_QMD/
    Skill_Harden/
    Tasker_Delegate/
    Tasker_Plan/
  plugins/
```

## Current Contents

- `agents-codex/.agents/plugins/marketplace.json` - Codex marketplace metadata for the Gymnasium plugin catalog.
- `agents-codex/skills/Code_Checkpoint/` - skill for creating frequent reversible git checkpoint commits.
- `agents-codex/skills/Code_Commit/` - skill for drafting crisp public-facing commit messages.
- `agents-codex/skills/Code_Develop/` - skill for implementing focused development work with verification.
- `agents-codex/skills/Code_Projects/` - skill for checking local project and clone locations before fetching.
- `agents-codex/skills/Search_QMD/` - skill for searching the vault with QMD.
- `agents-codex/skills/Skill_Harden/` - imported and adapted skill-authoring discipline from Abhinav Gupta's
  [`abhinav/home`](https://github.com/abhinav/home/tree/main/.agents/skills/writing-and-updating-skills);
  guidance for authoring and pressure-testing skills.
- `agents-codex/skills/Tasker_Delegate/` - skill for keeping the main thread lightweight while batching work through subagents.
- `agents-codex/skills/Tasker_Plan/` - skill for maintaining durable repo-root or brain-project plans.
- `agents-codex/plugins/` - placeholder for future Codex plugins.

See [`NOTICE.md`](NOTICE.md) for imported-material attribution.

## License

MIT. Some skill material is imported or adapted from
Abhinav Gupta's [`abhinav/home`](https://github.com/abhinav/home), which is also MIT-licensed.
