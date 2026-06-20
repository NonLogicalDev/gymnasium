---
date: 2026-06-16
status: complete
subject: product-integration
---

# Goal

Update `Tasker_Plan`, then still referred to as `build-with-plans`, so future medium-sized product, UI, and architecture work records how new requirements integrate into the whole product instead of defaulting to local additive patches.

# Context

The observed failure mode was incremental product drift: each request was implemented against the nearest existing surface, and verification proved the requested affordance existed without checking whether the overall product model still made sense.

# Product Integration

- Existing product model: plans captured goals, decisions, work logs, and unfinished work, but did not require a product-shape review.
- New requirement's real intent: force agents to preserve product coherence and consider re-architecture when new requirements strain the current model.
- Cleanest integrated model: add a required `Product Integration` planning pass for medium or larger product, UI, and architecture changes.
- Existing pieces that should move, change, or disappear: no existing plan sections are removed; `Product Integration` is added before `Decisions` so it informs implementation rather than documenting it afterward.
- Architecture impact: planning skill behavior changes; downstream project plans should now explain product-model fit before meaningful source edits.
- Why this is better than a local patch: it makes the clean integrated model explicit and catches short-term additive implementation before code is written.

# Decisions

- Add product-integration guidance to the skill description, core rules, required plan shape, workflow, and completion criteria.
- Add a reusable behavioral scenario that pressures the agent to add nested UI quickly and expects a product integration pass instead.
- Add matching global agent guidance so the behavior applies outside formal plan-skill use as well.

# Implementation Steps

- [x] Add product-integrity guidance to the global agent instructions.
- [x] Add product-integration requirements to `build-with-plans`.
- [x] Add a behavioral scenario for additive UI pressure.
- [x] Review diffs for scope and clarity.

# Learning Log

- Product integrity needs to be treated as a planning invariant, not a retrospective review comment.
- The dangerous shortcut is not only skipping plans; it is writing plans that preserve the nearest existing structure without asking if that structure still fits.

# Work Log

- [x] 2026-06-16 09:17 - Added the product-integration repair plan after drafting the instruction and skill changes.

# Unfinished Work

- None.
