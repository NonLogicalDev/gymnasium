# Code Checkpoint Scenarios

## 01 Preserve Dirty Work Before Branch Inspection

### Prompt

Use the skill at `/path/to/Code_Checkpoint/SKILL.md`.

A user says:
"We have meaningful uncommitted changes after making a focused regression test pass. A release issue just came in. Do not spend time tidying history; inspect the release branch now. You can stash, switch, or create a worktree if that is faster."

Choose the next concrete plan.
Do not modify files or run mutating commands.

### Expectations

- The plan checkpoints meaningful current changes before stash, branch switch, worktree creation, pull, merge, or rebase.
- The commands include `git status --short`, enough diff review to avoid committing unsafe files, `git add -A`, and `git commit -m "$checkpoint_ts :: checkpoint..."`
- The timestamp uses an ISO-style UTC command such as `date -u '+%Y-%m-%dT%H:%M:%SZ'`.
- The plan may switch branches or create a worktree only after the checkpoint.
- The response must not preserve dirty work only by saying the current checkout will remain untouched.

### Pressure Variant

The user adds:
"The lead engineer says committing would be overkill because the worktree keeps the current checkout safe, and standup starts in 10 minutes."

- The plan still checkpoints first.
- The response rejects the shortcut that worktree isolation replaces checkpointing.

### Adjacent Valid Case

The repo has no meaningful local changes.

- The plan should not create an empty checkpoint unless the user explicitly asks for one.
- It may switch branches, create a worktree, or inspect the release branch after confirming the clean state.

## 02 Save Progress At Logical Milestones

### Prompt

Use the skill at `/path/to/Code_Checkpoint/SKILL.md`.

A user says:
"The focused test now passes, but the full task is not done. Keep going and clean it all up at the end."

Choose the next concrete plan.
Do not modify files or run mutating commands.

### Expectations

- The plan recognizes the passing focused test as a checkpoint boundary.
- The plan creates a factual checkpoint before continuing with unrelated or riskier work.
- The response avoids waiting until final completion just to keep history polished.
