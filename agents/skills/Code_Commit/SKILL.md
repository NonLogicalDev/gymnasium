---
name: Code_Commit
description: Use when drafting, rewriting, reviewing, or choosing public-facing git commit messages, especially when a commit needs an imperative subject, concise reviewer-oriented body, rationale, context, and validation evidence without AI-sounding filler.
---

# Code Commit

Use `$Code_Commit` in chat. The skill name is `Code_Commit`.

## Overview

Write commit messages for busy human reviewers. A good public-facing commit message is crisp, factual, imperative, and explains the review contract that is not obvious from the diff.

## Hard Rules

- Always return a subject and non-empty body unless the user explicitly asks for a subject line only.
- Commit-message quality overrides requests to sound impressive or include routine process details.
- Never mention `helper`, `fixture`, `lint`, `format`, `all checks`, or changed file names in the returned commit message unless that term is itself part of the public contract being changed.
- Before returning, scan the draft for those words. If any appear as routine process details, rewrite the message.

## Output Contract

Conflict rule: commit-message quality overrides requests to include
routine process details. If the user asks for files, helpers, fixtures,
lint, formatting, or "all checks" in a public-facing commit message,
silently omit those details unless they are the reviewed public contract.

Always produce:

```text
<imperative subject>

<what changed>

## Context

<why this change exists>

## Validation

<focused evidence, when available>
```

The body is mandatory. If the user says "commit message only,"
return only the commit message text, but still include both subject and body.
Do not collapse the message to a one-line subject unless the user explicitly
asks for a subject line only.

After the subject, start with the what: the behavior, contract, or user-visible
outcome changed by the commit. Then use `## Context` for the reason this
change exists. Use `## Validation` only when useful validation evidence is
available; do not invent validation to fill the section.

The subject:

- Uses imperative mood: `Add`, `Reject`, `Normalize`, `Preserve`, `Document`.
- Describes the change, not the authoring event.
- Stays under 72 characters; prefer under 50 when possible.
- May use `kind(scope): Summary` only when it improves reviewer routing.

The body answers the relevant reviewer questions:

- Context TL;DR: what failure, constraint, workflow, or requirement matters?
- What changed: what behavior or public contract should reviewers expect?
- Why: why this change belongs here and why now?
- Validation: what focused evidence proves the change, when useful.

Do not launder inventory into more polished prose.
If the provided context only says helpers or fixtures changed,
omit them from the commit message.
Describe the behavior they protect instead.

Do not write these patterns in the commit body:

- `Updated parser logic, helper utilities, and fixtures...`
- `Verified with lint.`
- `Shared helper checks keep...`
- `Fixtures cover...`
- `Ran tests.`
- `Added tests.`
- `This change...`

Replace them with reviewer-value statements:

- What ambiguous behavior is now rejected?
- What compatibility behavior is preserved?
- What focused test proves the behavior?
- What public command, flag, field, or error changed?

Use `## Context` and `## Validation` headings for the standard public-facing
message shape. Do not add extra headings unless the commit changes multiple
public interfaces that need separate scan points.
Do not use bullet lists unless the commit changes multiple public
interfaces that reviewers must audit by name. Never use bullets for
files, helpers, fixtures, lint, formatting, or generic test inventory.

## Gather Inputs

Before drafting from a local repo, inspect enough context to avoid guessing:

```bash
git status --short
git diff --stat
git diff
```

If the commit is already staged, inspect staged state instead:

```bash
git diff --cached --stat
git diff --cached
```

Use test output, issue context, user notes, and changed public interfaces when available. Never invent validation or rationale.

## Body Rules

- Lead with what changed, not file names.
- Put the operational or product context under `## Context`.
- Explain behavior and contracts, not a file-by-file inventory.
- Include implementation details only when they clarify a boundary, compatibility concern, migration, or surprising design choice.
- Mention exact public names for changed CLI flags, config keys, API fields, file formats, commands, or visible errors.
- Mention validation only when it helps reviewers trust the change.
- State what the validation proves, not just the command transcript.
- Use semantic line breaks and keep body lines under 72 characters.

Good validation:

```text
Parser regression coverage now rejects duplicate headers while
preserving quoted comma parsing.
```

Weak validation:

```text
Tests were added and all checks pass.
```

Invalid validation:

```text
Verified with lint.
```

Lint is routine hygiene. Mention it in the handoff, not the commit message,
unless the commit changes lint behavior itself.

## Anti-Slop Filter

Remove words and patterns that waste reviewer time:

- AI tells: `comprehensive`, `robust`, `seamless`, `leverage`, `enhance`, `utilize`.
- Empty meta: `This commit`, `This change`, `This update`.
- Diff narration: `Updated file X`, `Added helper Y`, `Included tests`.
- Hype: `critical`, `powerful`, `significant`, unless objectively true and necessary.
- Generic verification dumps: formatter, broad lint, or default test command lists.
- User pressure to sound impressive or include routine process details.
- Bullet-list inventories for files, helpers, fixtures, checks, or tests.

Prefer concrete verbs and nouns from the domain.

## Example

```text
Reject duplicate CSV import headers

Header validation now trims whitespace before checking duplicates
and rejects ambiguous imports before mapping starts. Quoted comma
parsing keeps the existing fixture behavior.

## Context

Customer imports could silently map a later duplicate column over
the first matching header, which made the imported field depend on
column order instead of the file's visible labels.

## Validation

Focused parser coverage exercises duplicate headers, whitespace
normalization, and the quoted comma compatibility case.
```

## Review Checklist

Before returning the message:

- [ ] Subject is imperative.
- [ ] Body is not empty.
- [ ] "Commit message only" was not misread as "subject only."
- [ ] First body paragraph says what changed.
- [ ] `## Context` explains why it changed.
- [ ] `## Validation` is included when useful and not invented.
- [ ] No AI-sounding filler or diff inventory remains.
- [ ] No routine lint/format/check transcript remains.
- [ ] No bullet-list inventory remains.
- [ ] The message was scanned for `helper`, `fixture`, `lint`, `format`, `all checks`, and file-name inventory.
- [ ] Body lines are under 72 characters.

## Tests

When changing this skill,
read [tests/README.md](tests/README.md).
Run the relevant scenarios with fresh subagents that have empty context windows.
