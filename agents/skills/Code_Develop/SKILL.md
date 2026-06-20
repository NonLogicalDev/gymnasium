---
name: Code_Develop
description: Use when implementing or iterating on a development goal, feature, fix, refactor, or plan where the agent should stay true to the goal, ship an initial pass, then run adversarial review and fix cycles until the result is the smallest reasonable, well-structured, simplest solution.
---

# Code Develop

Use `$Code_Develop` in chat. The skill name is `Code_Develop`.

## Overview

Develop toward the user goal in tight iterations.
Do not stop at the first implementation.
After the first working pass,
challenge the result,
cut unnecessary scope,
repair weak structure,
and repeat until further churn is no longer justified.

## Core Invariant

Stay faithful to the requested goal while optimizing for:

- the smallest scope that fully solves the goal
- the simplest design that still fits the real constraints
- structure that is easy to explain and maintain
- evidence that each extra change is still earning its keep

The first implementation is a draft,
not the default final answer.

## Development Loop

1. Lock the target.
   - State the goal, constraints, and non-goals in concrete terms.
   - Identify what success must preserve.
2. Build the first reasonable pass.
   - Choose the narrowest implementation likely to satisfy the goal.
   - Avoid speculative abstraction on the first pass.
3. Run adversarial review on your own result.
   - Assume the implementation is too broad, too coupled, or too clever until disproven.
   - Look for a smaller, simpler, clearer shape that still satisfies the goal.
4. Fix the most important weakness.
   - Remove unnecessary surface area.
   - Collapse accidental indirection.
   - Tighten naming, ownership, and control flow.
   - Remove code that exists only to support abandoned branches of the design.
5. Re-evaluate.
   - If the result is still carrying avoidable complexity, repeat the review/fix cycle.
   - Stop only when the remaining complexity is justified by the actual goal or constraints.

## Adversarial Review Questions

After the first pass,
and after every meaningful rewrite,
ask:

- Can the same goal be met with fewer moving parts?
- Did I add abstraction before proving it was needed?
- Did I preserve structure that exists only because of the first implementation shape?
- Is any branch, helper, type, config, or layer solving a hypothetical future instead of today's goal?
- Can ownership move to one clearer place?
- Can two concepts collapse into one without losing clarity?
- Am I defending sunk cost instead of defending a requirement?
- If I designed this fresh now that I understand the problem better, would I keep this shape?

## What To Simplify First

Prefer removing:

- speculative abstractions
- generic helpers introduced for one call site
- indirection layers with no real boundary
- configuration that encodes one current behavior
- duplicate state or duplicated validation paths
- temporary compatibility code that no longer protects a real contract
- broad refactors that are no longer needed after the narrower fix becomes clear

Prefer keeping:

- explicit constraints the user actually asked for
- compatibility behavior that is still exercised
- structure that protects a real ownership boundary
- small local duplication when abstraction would make the code harder to follow

## Stop Condition

Stop iterating when all of these are true:

- the current solution satisfies the real goal
- the next simplification would risk correctness, clarity, or an explicit constraint
- the remaining complexity is tied to a real requirement, not inertia from earlier drafts
- you can explain why each remaining moving part still exists

Do not keep polishing for style after the structural issues are gone.

## Red Flags

| Red flag | Correct action |
| --- | --- |
| "It works, so stop here." | Run at least one adversarial review pass after the first implementation. |
| "I already wrote this abstraction." | Re-evaluate whether the abstraction still earns its keep. |
| "This extra layer may help later." | Remove it unless a current constraint requires it. |
| "The broader refactor is already half done." | Cut back to the smallest scope that solves the goal now. |
| "I can clean it up in a later pass." | Do the immediate structural cleanup before handoff when it materially simplifies the result. |
| "A more direct design would require rewriting the first pass." | Prefer the clearer shape when the rewrite is still contained and justified. |

## Adjacent Valid Cases

This skill does not require endless churn.
If the first pass is already the narrowest reasonable solution,
the adversarial review may conclude that no structural rewrite is needed.

This skill does not license unbounded redesign.
Stay anchored to the requested goal,
current repo constraints,
and the smallest scope that solves the task.

## Tests

When changing this skill,
read [tests/README.md](tests/README.md).
Run the relevant scenarios with fresh subagents that have empty context windows.
