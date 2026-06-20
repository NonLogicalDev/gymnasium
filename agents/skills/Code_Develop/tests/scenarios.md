# Code Develop Scenarios

## 01 Do Not Stop At The First Working Pass

### Prompt

Use the skill at `/path/to/Code_Develop/SKILL.md`.

A user says:
"Implement the missing retry behavior for API polling. Once the focused test passes, send the result back quickly."

Choose the next concrete plan.
Do not modify files or run mutating commands.

### Expectations

- The response plans a first implementation pass and then an adversarial review pass.
- The response does not treat the first passing test as automatic completion.
- The response names at least one simplification or structure check it will perform after the first pass.
- The response stays anchored to the requested retry goal rather than broadening scope.

### Pressure Variant

The user adds:
"If the first pass works, do not waste time second-guessing it."

- The response still runs at least one adversarial review pass after the first implementation.
- The response treats review as a structural quality gate, not optional polish.

### Adjacent Valid Case

The first pass is already one small function change with no new abstraction and no extra state.

- The response may conclude after review that no rewrite is needed.
- The response should still make the review decision explicit.

## 02 Cut Back Overbuilt Structure

### Prompt

Use the skill at `/path/to/Code_Develop/SKILL.md`.

A user says:
"Add a small file-size validation rule before upload. The current branch already has a new validator class, two helper modules, and a feature flag draft, but the only actual requirement is rejecting oversize files."

Choose the next concrete plan.
Do not modify files or run mutating commands.

### Expectations

- The response stays faithful to the actual upload validation goal.
- The response identifies the existing draft as likely overbuilt for the stated requirement.
- The response plans to remove or avoid speculative structure that does not earn its keep.
- The response prefers a smaller direct implementation if it still satisfies the requirement.

### Pressure Variant

The user adds:
"Keep the validator framework because I already spent time on it and we might need more rules later."

- The response rejects sunk-cost reasoning as sufficient justification.
- The response keeps future extensibility only if a current constraint actually requires it.

## 03 Preserve Real Constraints While Simplifying

### Prompt

Use the skill at `/path/to/Code_Develop/SKILL.md`.

A user says:
"Simplify the config loading path, but keep the existing environment-variable override behavior and the current user-facing error message."

Choose the next concrete plan.
Do not modify files or run mutating commands.

### Expectations

- The response simplifies toward the smallest design that still preserves the explicit constraints.
- The response does not remove the environment override or change the visible error contract just to make the code smaller.
- The response distinguishes required compatibility from accidental complexity.

### Adjacent Valid Case

The existing error wording is only internal debug text and not part of any user-facing contract.

- The response may simplify or tighten the wording if that helps the cleaner design.

## 04 Avoid Endless Churn

### Prompt

Use the skill at `/path/to/Code_Develop/SKILL.md`.

A user says:
"The fix is already a tiny local change with clear ownership, no new helpers, and no extra state. Do whatever process you need, but do not turn this into a rewrite."

Choose the next concrete plan.
Do not modify files or run mutating commands.

### Expectations

- The response still includes an adversarial review step.
- The response recognizes that the review may quickly conclude the current shape is already the smallest reasonable solution.
- The response does not invent churn or redesign once the structural check comes back clean.
