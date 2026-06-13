---
name: git-checkpoints
description: Use when working in a git repository with meaningful local edits, before risky git operations, before switching branches or contexts, after completing a logical milestone, or when the user asks to checkpoint, save progress, preserve work, or avoid losing changes.
---

# Git Checkpoints

## Overview

Use small chronological git commits as reversible checkpoints while work is in progress. A checkpoint is a recovery point and work log, not a polished final commit.

## Core Principle

Cut a checkpoint whenever undo ability or chronology is more useful than a perfectly curated history. Checkpoint commits can be squashed, reordered, or rewritten later.

## Iron Rule

If `git status --short` shows meaningful work that should survive, checkpoint it before any branch switch, worktree, pull, merge, rebase, risky edit, or context switch.

Leaving dirty changes "untouched" in the current checkout is not enough. The work is still uncommitted, easy to lose, and absent from the chronological record.

This rule takes priority over generic worktree-isolation habits: preserve the current dirty state first, then isolate follow-up work if useful.

Required decision boundary: when meaningful changes are uncommitted, the next plan must include a checkpoint before any alternate preservation or isolation strategy. Stash, worktree, branch switch, pull, merge, or rebase may follow; they cannot be the first preservation step.

Checkpoint after each logically important goal:

- A test starts passing.
- A bug is isolated or fixed.
- A refactor step is complete.
- A risky edit is about to start.
- A branch switch, pull, merge, rebase, or context switch is next.
- Changes would otherwise be stashed.
- A worktree or alternate checkout will be used to inspect another branch.
- The session is ending with meaningful changes.

Do not wait for the whole task to be done. The goal is to let the agent move forward without fear of losing work and to give the human a small-step record of how the result was reached.

## Checkpoint Command

Do not assume local aliases such as `git checkpoint` or `git save` exist.

Use the alias-equivalent command directly:

```bash
checkpoint_ts="$(date -u '+%Y-%m-%dT%H:%M:%SZ')"
git add -A
git commit -m "$checkpoint_ts :: checkpoint"
```

Add a short suffix when it helps the chronological story:

```bash
checkpoint_ts="$(date -u '+%Y-%m-%dT%H:%M:%SZ')"
git add -A
git commit -m "$checkpoint_ts :: checkpoint :: tests pass for parser edge cases"
```

Use UTC ISO-style timestamps so checkpoint messages sort predictably and do not depend on local `date` formatting.

Keep suffixes factual and short. Do not write polished release-style messages for checkpoint commits.

## Workflow

1. Inspect repo guidance first: `AGENTS.md`, `CLAUDE.md`, or equivalent.
   - If the repo defines a checkpoint command, use it.
   - If it forbids direct commits or uses another VCS, follow repo rules.
2. Check current state.
   - Run `git status --short`.
   - If there are no changes, do not create an empty checkpoint unless explicitly asked.
3. Review enough diff to avoid committing secrets, generated junk, or unrelated destructive edits.
   - Use `git diff --stat`; use targeted `git diff` when needed.
   - Do not revert user changes just to make the checkpoint cleaner.
4. Create the checkpoint with the alias-equivalent command.
5. Report the short hash and why the checkpoint was cut.

## Branch Or Worktree Example

When a test passes and the user asks to inspect another branch, checkpoint first:

```bash
git status --short
git diff --stat
checkpoint_ts="$(date -u '+%Y-%m-%dT%H:%M:%SZ')"
git add -A
git commit -m "$checkpoint_ts :: checkpoint :: parser regression test passes"
```

Then switch branches or create a worktree if that is still the right approach. Do not create the worktree first.

## Use Instead Of Stash

Before switching branches, pulling, rebasing, creating a worktree to inspect another branch, or doing work that would normally require `git stash`, prefer a checkpoint:

```bash
git status --short
checkpoint_ts="$(date -u '+%Y-%m-%dT%H:%M:%SZ')"
git add -A
git commit -m "$checkpoint_ts :: checkpoint :: before switching branches"
```

Checkpoint commits are easier to inspect, recover, and merge later than anonymous stashes. Use `git stash` only when repo instructions require it or the human asks.

Do not route around checkpointing by creating a worktree first. Worktrees are useful, but they do not preserve the current dirty checkout in git history. If local changes are meaningful, checkpoint them before switching strategy.

## Frequency

Checkpoint at boundaries, not every keystroke:

- "Initial scaffold exists."
- "Regression test reproduces bug."
- "Implementation passes focused test."
- "Renamed files and updated references."
- "Before dependency upgrade."
- "Before switching away with dirty worktree."

## Common Mistakes

| Mistake | Correction |
|---------|------------|
| Relying on `git checkpoint` alias | Use `git add -A` plus `git commit -m "$checkpoint_ts :: checkpoint..."` unless the repo defines an alias. |
| Waiting until the final answer | Checkpoint after each meaningful milestone. |
| Using `git stash` by habit | Prefer a checkpoint commit before branch/context switches. |
| Creating a worktree to avoid dirty-state handling | Checkpoint meaningful current changes first; then create the worktree if still useful. |
| Saying dirty changes are safe because they are untouched | They are still uncommitted; checkpoint them before context switches. |
| Making polished commits too early | Use factual checkpoint messages; curate later if desired. |
| Committing blindly | Inspect status and relevant diffs first. |
| Reverting user changes | Preserve them; checkpoint or ask if they conflict with the task. |

## Safety Rules

- Never run destructive commands such as `git reset --hard`, broad `git checkout`, or `git restore` to make checkpointing easier.
- Never checkpoint secrets, credentials, or private keys.
- Avoid checkpointing half-written files that cannot parse unless stopping immediately.
- Separate unrelated work first when it is cheap and safe.
- Never switch branches before preserving or explicitly resolving local changes.
- If committing is blocked by hooks or policy, report the failure and current `git status --short`.
- If the repository is not using git, do not invent an equivalent unless repo instructions define one.

## Tests

When changing this skill,
read [tests/README.md](tests/README.md).
Run the relevant scenarios with fresh subagents that have empty context windows.
