# Gymnasium

Gymnasium is a local curation repo for agent skills and plugins.

The initial focus is Codex, with room to adapt useful patterns to other agent runtimes later.

## Layout

```text
agents/
  skills/
    Code_Checkpoint/
    Code_Commit/
    Code_Develop/
    Code_Projects/
    Search_QMD/
    Skill_Harden/
    Tasker_Delegate/
    Tasker_Plan/
agents-codex/
  README.md
  collection/
    .agents/plugins/marketplace.json
    plugins/
agents-claude/
  CLAUDE.md
  collection/
    .claude-plugin/marketplace.json
    plugins/
```

## Current Contents

- `agents/skills/Code_Checkpoint/` - skill for creating frequent reversible git checkpoint commits.
- `agents/skills/Code_Commit/` - skill for drafting crisp public-facing commit messages.
- `agents/skills/Code_Develop/` - skill for implementing focused development work with verification.
- `agents/skills/Code_Projects/` - skill for checking local project and clone locations before fetching.
- `agents/skills/Search_QMD/` - skill for searching the vault with QMD.
- `agents/skills/Skill_Harden/` - imported and adapted skill-authoring discipline from Abhinav Gupta's
  [`abhinav/home`](https://github.com/abhinav/home/tree/main/.agents/skills/writing-and-updating-skills);
  guidance for authoring and pressure-testing skills.
- `agents/skills/Tasker_Delegate/` - skill for keeping the main thread lightweight while batching work through subagents.
- `agents/skills/Tasker_Plan/` - skill for maintaining durable repo-root or brain-project plans.
- `agents-codex/` - Codex-specific plugin collection and future Codex runtime material.
- `agents-codex/collection/.agents/plugins/marketplace.json` - Codex marketplace metadata for the Gymnasium plugin catalog.
- `agents-codex/collection/plugins/` - placeholder for future Codex plugins published through the Codex collection.
- `agents-claude/` - native Claude Code plugin marketplace; aimed at reducing papercuts and stabilizing long-running Claude agents.
- `agents-claude/collection/.claude-plugin/marketplace.json` - Claude Code marketplace manifest (`gymnasium-claude`).
- `agents-claude/CLAUDE.md` - curated guidance for stabilizing long-running Claude agents.
- `agents-claude/collection/plugins/` - placeholder for future Claude plugins.

See [`NOTICE.md`](NOTICE.md) for imported-material attribution.

## License

MIT. Some skill material is imported or adapted from
Abhinav Gupta's [`abhinav/home`](https://github.com/abhinav/home), which is also MIT-licensed.
