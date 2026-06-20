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
- Cleanest integrated model: the skill owns one helper, `scripts/qmd-vault-helper`, with explicit `init` and `exec` modes so setup and upstream QMD forwarding are separate commands.
- Existing pieces that should move, change, or disappear: remove the old short `qmd` helper, stop documenting vault-local helper paths, and update vault-facing instructions to call the standalone Gymnasium helper.
- Architecture impact: low to medium; this changes Search_QMD's invocation boundary and helper runtime behavior, but not the upstream QMD index model.
- Why this is better than a local patch: it prevents future agents from guessing vault roots or relying on whichever symlink layout happens to exist on one machine.

# Decisions

- Use `agents/skills/Search_QMD/scripts/qmd-vault-helper` as the canonical helper path from the Gymnasium repo root.
- Use `scripts/qmd-vault-helper` when already inside the Search_QMD skill folder.
- Use `qmd-vault-helper <helper-args> init <helper-args>` for setup and `qmd-vault-helper <helper-args> exec <qmd-args>` for QMD execution.
- Require `--vault-root` or `QMD_VAULT_ROOT`; do not infer the vault root from cwd, git root, symlink path, or helper location.
- Support `--invoke-method` / `QMD_INVOKE_METHOD` as a single method or comma-separated preference list.
- Invocation methods are `local`, `bunx`, `nix`, and explicit opt-in `bun-bundle`; default is `local,bunx,nix`.
- Rename the compiled-Bun method to `bun-bundle` and add `bunx` as a separate package-runner method.
- Keep `bun-bundle` out of the default method list because compiled QMD executables built by Bun currently fail their smoke test on this host.
- When no `--package-root` is provided for `bun-bundle`, install the package into a cache-owned temporary source directory, compile the actual package entrypoint from there, remove the source, and reuse the compiled executable.
- Use uv standalone script metadata plus `click` for option parsing and `rich` for readable `init` output.
- Keep helper text runtime-agnostic; do not make Codex-specific path assumptions in the helper.
- Do not restore `.agents/scripts/qmd` as a compatibility wrapper.
- Replace the `--` separator contract with explicit helper subcommands: `qmd-vault-helper <helper-args> init <helper-args>` for setup and `qmd-vault-helper <helper-args> exec <qmd-args>` for QMD execution.
- Rewrite the helper as a uv standalone Python script because the bash implementation became too complex for reliable maintenance.

# Implementation Steps

1. Rename `agents/skills/Search_QMD/scripts/qmd` to `qmd-vault-helper`.
2. Add helper argument parsing at the top level and on `init`.
3. Add an agent-readable `--help` page and preserve upstream QMD help through `exec --help`.
4. Rework vault-root handling to require explicit `--vault-root` or `QMD_VAULT_ROOT`.
5. Add invocation methods for `local`, `bun-bundle`, `bunx`, and `nix`.
6. Update Search_QMD skill documentation to use standalone Gymnasium paths and the `init` / `exec` split.
7. Update vault `AGENTS.md` QMD instructions to call the Gymnasium helper.
8. Validate the helper contract and checkpoint the repo.
9. Convert the helper from bash to a uv Python script.
10. Update Search_QMD and vault-facing docs to the `init` / `exec` command split.
11. Retest helper help, init, exec, method fallback, and error behavior.

# Learning Log

- `bun build --compile` can produce a QMD executable on this host, but Bun-compiled executables exit `137`. The helper now smoke-tests `bun-bundle` output and can continue to later methods when the invocation method is a list.
- A helper-owned `--help` page is part of the behavior contract, not just documentation, because agents need to discover required args before knowing the env setup.
- The old helper name `qmd` made the wrapper look like the upstream command; `qmd-vault-helper` makes the wrapper boundary explicit.
- Keeping QMD mutable state under `<vault>/.qmd-agent` remains the right default for sandboxed agent runs.
- The explicit `init` / `exec` split is clearer than `--` because it makes setup and QMD forwarding two named modes instead of relying on a positional separator.
- Bun's documented compile flow and local CLI help both require a local entrypoint; `bun build @tobilu/qmd --compile` treats the package name as a filesystem path and fails. The helper therefore materializes package source before compiling.
- `bun-bundle` build/install stdout should stay out of `exec` output because agents may request QMD JSON output.

# Work Log

- [x] 2026-06-20 15:09 - Created this standalone epic after the Search_QMD helper work outgrew the older skill rename-alignment plan.
- [x] 2026-06-20 15:09 - Captured the earlier root-resolution checkpoint: `ee74127`.
- [x] 2026-06-20 15:09 - Captured the helper-contract checkpoint: `2d10960`.
- [x] 2026-06-20 15:09 - Recorded the first helper-contract decisions before the later `init` / `exec` rewrite.
- [x] 2026-06-20 15:09 - Recorded validation coverage from syntax checks, helper help, QMD help pass-through, parser failure checks, fake local, fake bunx, and fake bun-bundle smoke tests.
- [x] 2026-06-20 15:12 - Reopened the plan for the new `init` / `exec` helper command split.
- [x] 2026-06-20 15:24 - Rewrote `qmd-vault-helper` as a uv standalone Python script using `click` and `rich`.
- [x] 2026-06-20 15:24 - Updated Search_QMD and vault AGENTS docs to `qmd-vault-helper <helper-args> init <helper-args>` and `qmd-vault-helper <helper-args> exec <qmd-args>`.
- [x] 2026-06-20 15:24 - Added cache-owned temporary package source materialization for `bun-bundle` when `--package-root` is omitted.
- [x] 2026-06-20 15:24 - Retested helper behavior after the Python rewrite.
- [x] 2026-06-20 15:25 - Prepared the Gymnasium checkpoint with the finalized helper, docs, and plan updates.

# Unfinished Work

- [x] Rewrite helper as uv Python while preserving vault-scoped QMD state behavior.
- [x] Update docs to the `init` / `exec` command split.
- [x] Retest syntax, help, init, exec, and invocation method behavior.
- [x] Checkpoint the Gymnasium repo.

# Validation

- `PYTHONPYCACHEPREFIX=/private/tmp/qmd-helper-pycache python3 -m py_compile agents/skills/Search_QMD/scripts/qmd-vault-helper` passed.
- `UV_CACHE_DIR=/private/tmp/qmd-helper-uv-cache agents/skills/Search_QMD/scripts/qmd-vault-helper --help` prints helper-owned usage without vault setup.
- `qmd-vault-helper --vault-root <vault> init` and `qmd-vault-helper init --vault-root <vault>` both initialize vault-scoped QMD state.
- `qmd-vault-helper --vault-root /Users/nonlogical/Notes/Personal-OBS exec --help` passes through to upstream QMD help.
- Missing `--vault-root` fails with an agent-readable helper error.
- Fake local and fake `bunx` method tests passed.
- Fake `bun-bundle` tests passed for explicit `--package-root` and omitted package root with temporary source materialization plus compiled-binary reuse.
- `bun build @tobilu/qmd --compile --outfile /private/tmp/qmd-direct-npm-test` fails by treating the package name as a path, confirming that source materialization is required.
- Static scans found no stale vault-local helper path, old separator syntax, old default method order, or removed helper-option references in Search_QMD docs or vault AGENTS guidance.
