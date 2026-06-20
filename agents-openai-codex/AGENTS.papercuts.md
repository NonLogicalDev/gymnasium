# Codex Papercuts

These are small but firm operating rules for Codex-specific guidance and generated repo artifacts.

## Papercut 1: No Absolute User Paths

Codex should not write absolute paths into repo guidance, plans, docs, examples, commands, tests, or generated artifacts when those paths point into a user's home directory.

Use `~` conventions instead:

- Prefer `~/brain` over an expanded home-directory path.
- Prefer `~/Projects/local/<repo>` over an expanded home-directory path.
- Prefer `~/Projects/remote/github.com/<owner>/<repo>` over an expanded home-directory path.

Absolute paths are acceptable only when the user explicitly asks for a machine-local one-off path, a tool requires an absolute path for execution, or the path is being reported as command output. Do not persist those absolute user paths back into reusable repo files unless the file is explicitly machine-local.

## Papercut 2: Repos Are Isolated

Cross-repo linking is strongly discouraged. Treat every repo as isolated by default.

Do not make one repo's reusable guidance depend on another repo's private checkout path, local branch, generated file, or internal docs. If context must be shared across projects or repos, route it through `~/brain`.

Allowed cross-boundary references:

- `~/brain` for broader project and agent memory.
- Public upstream URLs when a repo intentionally depends on an external source.
- Runtime/package references that are part of the repo's declared dependency model.

When in doubt, keep repo-local instructions repo-local and put cross-project context in `~/brain`.
