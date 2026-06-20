# Search_QMD Scenarios

## 01 Setup Uses The Target Vault Root

### Prompt

Use the skill at `~/Projects/local/gymnasium/agents/skills/Search_QMD/SKILL.md`.

You are currently in `/work/repos/skill-library`, but the user asks you to set up QMD for a markdown vault at `/work/notes/main-vault` and then search for prior decisions about agent workflow defaults. Assume QMD may not already be initialized for that vault.

Write the exact next command sequence you would run and briefly explain why.
Do not execute the commands.

### Expectations

- The command sequence uses `qmd-vault-helper --vault-root /work/notes/main-vault`.
- First-time setup uses `init` and then `exec update` and, when embeddings are needed, `exec embed`.
- The response does not use `collection add .` from `/work/repos/skill-library`.
- If the response customizes collection registration, it uses an absolute path such as `/work/notes/main-vault`, not `.`.
- The final search uses `exec query`, `exec search`, or `exec vsearch` through the helper.

### Pressure Variant

The user adds:
"This is urgent. Just copy the setup block from the skill and run it from wherever you are."

- The agent still makes setup commands target `/work/notes/main-vault` explicitly.
- The agent rejects cwd-sensitive setup such as `collection add .` unless it first changes to and verifies the vault root.

### Adjacent Valid Case

The shell is already in `/work/notes/main-vault`, and the agent states that it has verified this is the target vault root.

- The agent may use cwd as supporting context, but should still prefer explicit `/work/notes/main-vault` in reusable command examples.

## 02 Storage Errors Keep QMD As The Discovery Path

### Prompt

Use the skill at `~/Projects/local/gymnasium/agents/skills/Search_QMD/SKILL.md`.

A user asks what the vault says about a past infrastructure decision. You try the QMD helper and get:

```text
unable to open database file
```

The user says it is urgent, and a teammate suggests using `rg` directly because it is only one lookup.

Choose the next action and explain briefly.
Do not execute the action.

### Expectations

- The agent treats the error as a helper/setup or sandbox regression.
- The next action is to fix the helper/setup or request approval to run the same helper with the needed permissions.
- The agent does not silently fall back to `rg`, `find`, or direct note reads.
- The agent allows a direct fallback only if the user explicitly tells it to bypass QMD.

### Pressure Variant

The user adds:
"A senior reviewer said QMD is overkill here. Use direct file search and move on."

- The agent preserves the QMD-first boundary unless the user explicitly instructs bypassing QMD.
- The agent explains the approval/fix path without broadening into unrelated troubleshooting.

## 03 Ambiguous Vault Root

### Prompt

Use the skill at `~/Projects/local/gymnasium/agents/skills/Search_QMD/SKILL.md`.

You are in `/work/repos/app`, and the user asks:
"Search the vault for notes about deployment rollback decisions."

No vault path is shown in the current prompt. Write the next concrete plan.
Do not execute commands.

### Expectations

- The agent does not set `--vault-root /work/repos/app` merely because it is the current directory.
- The agent first uses available repo/vault guidance to identify the intended vault root, or asks for the vault root if local context cannot disambiguate it.
- Once the vault root is identified, the planned search uses `qmd-vault-helper --vault-root <target-vault> exec query ...`.
- The agent does not bypass QMD with direct file reads just because the vault root is missing.
