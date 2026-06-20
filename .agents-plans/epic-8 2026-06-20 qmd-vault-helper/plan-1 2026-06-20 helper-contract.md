---
date: 2026-06-20
status: complete
subject: helper-contract
---

# Goal

Rework the Search_QMD helper so it is a standalone Gymnasium skill helper rather than a vault-local `.agents` wrapper, with a clear agent-readable invocation contract.

# Context

Search_QMD had outdated helper-path assumptions after Gymnasium moved shared skills under `agents/skills/`. The helper also inferred the vault root from the helper location or current working directory, which made it easy for agents to write QMD state into the wrong tree. During the rework, the desired shape changed from a simple path fix into a helper contract:

- Rename the helper from `qmd` to `qmd-vault-helper`.
- Require an explicit vault root.
- Support configurable QMD invocation methods.
- Keep the helper standalone from the Gymnasium repo, not tied to `.agents/skills/...` symlink layouts.
- Make `$SCRIPT --help` useful for agents.

# Product Integration

- Existing product model: Search_QMD is a reusable Gymnasium skill that agents can load directly from the repo, while vaults can reference it from their own AGENTS guidance.
- New requirement's real intent: make the helper self-describing, portable, and safe for agents to invoke across vaults without relying on hidden install-path conventions.
- Cleanest integrated model: the skill owns one helper, `scripts/qmd-vault-helper`, with helper arguments before `--` and upstream QMD arguments after `--`.
- Existing pieces that should move, change, or disappear: remove the old `scripts/qmd` helper, stop documenting `.agents/skills/Search_QMD/scripts/qmd`, and update vault-facing instructions to call the standalone Gymnasium helper.
- Architecture impact: low to medium; this changes Search_QMD's invocation boundary and helper runtime behavior, but not the upstream QMD index model.
- Why this is better than a local patch: it prevents future agents from guessing vault roots or relying on whichever symlink layout happens to exist on one machine.

# Decisions

- Use `agents/skills/Search_QMD/scripts/qmd-vault-helper` as the canonical helper path from the Gymnasium repo root.
- Use `scripts/qmd-vault-helper` when already inside the Search_QMD skill folder.
- Use `qmd-vault-helper <helper-args> -- <qmd-args>` as the convention so helper arguments and upstream QMD arguments cannot be confused.
- Require `--vault-root` or `QMD_VAULT_ROOT`; do not infer the vault root from cwd, git root, symlink path, or helper location.
- Support `--invoke-method` / `QMD_INVOKE_METHOD` as a single method or comma-separated preference list.
- Invocation methods are `local`, `bun-bundle`, `bunx`, and `nix`; default is `local,bun-bundle,bunx,nix`.
- Rename the compiled-Bun method to `bun-bundle` and add `bunx` as a separate package-runner method.
- Keep helper text runtime-agnostic; do not make Codex-specific path assumptions in the helper.
- Do not restore `.agents/scripts/qmd` as a compatibility wrapper.

# Implementation Steps

1. Rename `agents/skills/Search_QMD/scripts/qmd` to `qmd-vault-helper`.
2. Add helper argument parsing before `--`.
3. Add an agent-readable `--help` page and `--qmd-help` pass-through.
4. Rework vault-root handling to require explicit `--vault-root` or `QMD_VAULT_ROOT`.
5. Add invocation methods for `local`, `bun-bundle`, `bunx`, and `nix`.
6. Update Search_QMD skill documentation to use standalone Gymnasium paths and the `--` separator.
7. Update vault `AGENTS.md` QMD instructions to call the Gymnasium helper.
8. Validate the helper contract and checkpoint the repo.

# Learning Log

- `bun build --compile` can produce a QMD executable on this host, but Bun-compiled executables exit `137`. The helper now smoke-tests `bun-bundle` output and can continue to later methods when the invocation method is a list.
- A helper-owned `--help` page is part of the behavior contract, not just documentation, because agents need to discover required args before knowing the env setup.
- The old helper name `qmd` made the wrapper look like the upstream command; `qmd-vault-helper` makes the wrapper boundary explicit.
- Keeping QMD mutable state under `<vault>/.qmd-agent` remains the right default for sandboxed agent runs.

# Work Log

- [x] 2026-06-20 15:09 - Created this standalone epic after the Search_QMD helper work outgrew the older skill rename-alignment plan.
- [x] 2026-06-20 15:09 - Captured the earlier root-resolution checkpoint: `ee74127`.
- [x] 2026-06-20 15:09 - Captured the helper-contract checkpoint: `2d10960`.
- [x] 2026-06-20 15:09 - Recorded final invocation decisions: `qmd-vault-helper <helper-args> -- <qmd-args>`, explicit vault root, and `local,bun-bundle,bunx,nix` method ordering.
- [x] 2026-06-20 15:09 - Recorded validation coverage from syntax checks, helper help, QMD help pass-through, parser failure checks, fake local, fake bunx, and fake bun-bundle smoke tests.

# Unfinished Work

- [x] Rename the helper to `qmd-vault-helper`.
- [x] Update Search_QMD docs away from `.agents`-relative helper paths.
- [x] Update vault AGENTS guidance to the standalone helper path.
- [x] Validate helper argument parsing and invocation method behavior.
- [x] Checkpoint the Gymnasium repo.

# Validation

- `bash -n agents/skills/Search_QMD/scripts/qmd-vault-helper` passed.
- `agents/skills/Search_QMD/scripts/qmd-vault-helper --help` prints helper-owned usage without vault setup.
- `agents/skills/Search_QMD/scripts/qmd-vault-helper --vault-root <vault> -- --help` passes through to upstream QMD help.
- Missing `--` before QMD args fails with an agent-readable helper error.
- Fake local, fake `bunx`, and fake `bun-bundle` smoke tests passed.
- Static scans found no stale `.agents/scripts/qmd`, `.agents/skills/Search_QMD/scripts/qmd`, or old `bun` invocation-method references in Search_QMD docs or vault AGENTS guidance.
- Gymnasium commit `2d10960` contains the helper rework.
