# Codex Papercuts

These are small but firm operating rules for Codex-specific guidance and generated repo artifacts.

## Papercut 1: No Absolute User Paths

Codex should not write absolute paths into repo guidance, plans, docs, examples, commands, tests, or generated artifacts when those paths point into a user's home directory.

Use user-facing shorthands or resolvers instead:

- Prefer `$Agent_Brain` or its selected user-facing shorthand over hard-coded brain paths.
- Prefer `~/Projects/local/<repo>` over an expanded home-directory path.
- Prefer `~/Projects/remote/github.com/<owner>/<repo>` over an expanded home-directory path.

Absolute paths are acceptable only when the user explicitly asks for a machine-local one-off path, a tool requires an absolute path for execution, or the path is being reported as command output. Do not persist those absolute user paths back into reusable repo files unless the file is explicitly machine-local.

## Papercut 2: Agents Are Strongly Encouraged

Codex is explicitly requested, allowed, and strongly encouraged to use agents wherever they make sense.

Full permission is given, in the strongest wording, for Codex to run as many focused agents as required for the task. Treat this as standing user preference, not as a narrow one-off exception.

Use agents especially for:

- Independent repo exploration, audits, and validation.
- Parallel research tracks.
- Mechanical scans, checks, and comparisons.
- Focused implementation slices with disjoint write scopes.
- Pressure tests for skills and agent behavior.
- Long or noisy command workflows that should not clutter the main thread.

Coordinate agent work deliberately:

- Give each agent a narrow objective and expected return format.
- Keep write scopes disjoint when multiple agents edit files.
- Integrate and review agent results before treating the task as done.
- Close agents when they are no longer needed.

Do not treat agent use as optional ceremony. If agents would reduce context load, improve confidence, or make independent work faster, use them.

## Papercut 3: Repos Are Isolated

Cross-repo linking is strongly discouraged. Treat every repo as isolated by default.

Do not make one repo's reusable guidance depend on another repo's private checkout path, local branch, generated file, or internal docs. If context must be shared across projects or repos, route it through `$Agent_Brain`.

Allowed cross-boundary references:

- `$Agent_Brain` for broader project and agent memory.
- Public upstream URLs when a repo intentionally depends on an external source.
- Runtime/package references that are part of the repo's declared dependency model.

When in doubt, keep repo-local instructions repo-local and put cross-project context behind `$Agent_Brain`.

## Papercut 4: Global Agents File Means User Agents File

When Codex guidance says "global AGENTS.md" or "global agents file", it almost always means the user-level Codex agents file at `~/.codex/AGENTS.md`.

Do not confuse it with a repo-local `AGENTS.md`, a vault-level `AGENTS.md`, or generated project documentation. Before changing global agent behavior, verify whether the target is the user agents file; repo-local files should only override or augment behavior for their own checkout.
