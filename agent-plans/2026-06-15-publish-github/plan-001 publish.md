---
date: 2026-06-15
status: in-progress
subject: publish-github
---

# Goal

Publish `gymnasium` publicly under the `nonlogicaldev` GitHub account with an MIT license.

# Context

The repo is a local curation repo for Codex skills and plugins. It already has checkpoint history and a local Codex marketplace manifest. Publishing requires a public-safe license commit, GitHub remote creation, and push.

# Decisions

- Use MIT license with `nonlogicaldev` as the copyright holder.
- Keep existing checkpoint history unless the user asks to rewrite it.
- Use `gh` for GitHub auth, repository creation, and push setup if available.
- Checkpoint local changes before publishing.

# Implementation Steps

1. Confirm working tree, remotes, and GitHub CLI auth.
2. Run a public-readiness audit for secrets/junk/large files.
3. Add `LICENSE` and README license note.
4. Checkpoint the local license/publish metadata change.
5. Create `nonlogicaldev/gymnasium` on GitHub if needed.
6. Push `main` to the GitHub remote.
7. Verify the remote URL and clean local status.

# Learning Log

- Marketplace publication and GitHub publication are separate: the local Codex marketplace config is already present, while GitHub publication needs a remote repo and pushed history.
- Public-readiness audit found no obvious secrets, large files, or generated caches in repo contents/history.
- Imported `writing-and-updating-skills` material is compatible with MIT publication because upstream `abhinav/home` declares the repository contents MIT-licensed.

# Work Log

- [x] 2026-06-15 10:39 - Created this publish plan.
- [x] 2026-06-15 10:39 - Completed public-readiness audit.
- [x] 2026-06-15 10:39 - Added MIT license, upstream provenance note, and publish `.gitignore`.
- [x] 2026-06-15 10:45 - Completed GitHub device authorization for `gh`.
- [ ] 2026-06-15 10:45 - Checkpoint publish metadata.
- [ ] 2026-06-15 10:45 - Create and push GitHub repo.

# Unfinished Work

- [ ] Checkpoint publish metadata, create GitHub repo, push, and verify.
