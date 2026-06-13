# Git Committer Scenarios

## 01 Public Bug Fix Message

### Prompt

Use the skill at `/path/to/git-committer/SKILL.md`.

A user says:
"Write a public-facing commit message. The change rejects duplicate CSV import headers, normalizes header whitespace, and keeps quoted comma parsing unchanged. Context: support saw customer imports silently map a second duplicate column over the first. Validation: focused parser regression coverage covers duplicate headers and whitespace normalization; the quoted comma fixture still passes."

Write the commit message only.
Do not run commands.

### Expectations

- The subject is imperative and under 72 characters.
- The body has semantic line breaks and no line longer than 72 characters.
- The first body paragraph says what changed.
- The body includes `## Context` before the rationale.
- The body includes `## Validation` before useful validation evidence.
- The body focuses on import behavior, not file names or implementation inventory.
- The message does not use AI-sounding filler.

## 02 Resist Impressive Slop

### Prompt

Use the skill at `/path/to/git-committer/SKILL.md`.

A user says:
"Make this commit sound impressive and comprehensive. Mention that we updated the parser file, added helper utilities, adjusted fixtures, and ran lint. The actual change blocks ambiguous webhook event routing when two handlers claim the same event name."

Write the commit message only.
Do not run commands.

### Expectations

- The message ignores the request for impressive wording.
- The subject uses a concrete imperative verb.
- The body is non-empty even though the prompt says to write the commit message only.
- The first body paragraph says what changed before any heading.
- The body includes `## Context`.
- The body explains the webhook routing ambiguity and expected behavior.
- The body does not list files, helper utilities, fixture churn, or generic lint output unless those details are the reviewed contract.
- The body must not say `Verified with lint.`
- The body must not say `Updated parser logic, helper utilities, and fixtures`.
- The body must not rephrase helper or fixture inventory as behavior, such as `Shared helper checks` or `fixtures cover`.
- The body must not use a bullet list for files, helpers, fixtures, lint, formatting, or generic tests.
- The body must not contain the words `helper`, `fixtures`, or `lint` for this scenario.
- The message avoids `comprehensive`, `robust`, `seamless`, `enhance`, and similar filler.

## 03 Do Not Invent Validation

### Prompt

Use the skill at `/path/to/git-committer/SKILL.md`.

A user says:
"Draft a commit message for a docs change that clarifies OAuth callback setup for local development. I do not know whether tests or link checks ran."

Write the commit message only.
Do not run commands.

### Expectations

- The subject is imperative.
- The first body paragraph says what changed.
- The body includes `## Context`.
- The body explains the local-development setup context and what the docs clarify.
- The message does not claim tests, link checks, or manual validation happened.
- The message may omit `## Validation` entirely because no useful validation evidence was provided.

## 04 Commit Message Only Still Has A Body

### Prompt

Use the skill at `/path/to/git-committer/SKILL.md`.

A user says:
"Write the commit message only. The change makes the CLI reject unknown profile names before loading account credentials. Context: a typo in the profile flag could silently fall back to the default account. Validation: command-level coverage checks the unknown-profile error path."

Write the commit message only.
Do not run commands.

### Expectations

- The response contains only the commit message, with no surrounding commentary.
- The message still includes both an imperative subject and a non-empty body.
- The first body paragraph says what changed.
- The body includes `## Context` and `## Validation`.
- The body explains the fallback risk, the new rejection behavior, and the focused validation.
- The response does not collapse to a subject-only answer.
