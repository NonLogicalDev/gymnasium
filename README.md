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
agents-openai-codex/
  README.md
  collection/
    .agents/plugins/marketplace.json
    plugins/
agents-anthropic-claude/
  CLAUDE.md
  collection/
    .claude-plugin/marketplace.json
    plugins/
agents-google-antigravity/
  ANTIGRAVITY.md
  collection/
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
- `agents-openai-codex/` - Codex-specific plugin collection and future Codex runtime material.
- `agents-openai-codex/collection/.agents/plugins/marketplace.json` - Codex marketplace metadata for the Gymnasium plugin catalog.
- `agents-openai-codex/collection/plugins/` - placeholder for future Codex plugins published through the Codex collection.
- `agents-anthropic-claude/` - native Claude Code plugin marketplace; aimed at reducing papercuts and stabilizing long-running Claude agents.
- `agents-anthropic-claude/collection/.claude-plugin/marketplace.json` - Claude Code marketplace manifest (`gymnasium-claude`).
- `agents-anthropic-claude/CLAUDE.md` - curated guidance for stabilizing long-running Claude agents.
- `agents-anthropic-claude/collection/plugins/` - placeholder for future Claude plugins.
- `agents-google-antigravity/` - Google Antigravity scaffold for future plugins and runtime guidance.
- `agents-google-antigravity/collection/plugins/` - placeholder for future Antigravity plugins validated with `agy plugin validate`.

See [`NOTICE.md`](NOTICE.md) for imported-material attribution.

## License

MIT. Some skill material is imported or adapted from
Abhinav Gupta's [`abhinav/home`](https://github.com/abhinav/home), which is also MIT-licensed.
