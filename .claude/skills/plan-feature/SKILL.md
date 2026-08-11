---
name: plan-feature
description: Explore the code and write a self-contained SPEC.md before any edits. Use for features or changes that touch multiple files or where the approach is uncertain. Interviews the user, researches the codebase read-only, and produces a spec a fresh session can implement.
disable-model-invocation: true
---

# Plan a feature

Goal: turn a rough idea into a precise, self-contained spec — **without editing code**.
The value is in getting the problem right before writing the solution.

Stay in plan mode for this whole skill. Read files, run read-only commands, ask
questions. Do not edit files except the final `SPEC.md`.

## 1. Interview the user

Use the `AskUserQuestion` tool. Ask about the hard parts they may not have
considered — not the obvious ones:

- What does "done" look like? What is the observable behavior?
- Technical approach and any constraints (libraries allowed, performance, compat).
- UI/UX decisions, if any.
- Edge cases and failure modes.
- Explicitly: what is **out of scope**.

Keep interviewing until the ambiguity is gone. One round is rarely enough for a
real feature.

Use the vocabulary from CONTEXT.md in your questions and in the spec — precise
shared terms are what keep the spec unambiguous.

## 2. Explore the codebase

Delegate wide reading to the `explorer` subagent so raw file contents don't fill
this context:

> Use the explorer subagent to find how <the relevant subsystem> works and which
> files a change would touch.

Confirm which files change, the current patterns to follow, and reusable utilities
that already exist.

## 3. Write SPEC.md

Write a `SPEC.md` that a **fresh session with no memory of this conversation**
could implement. It must contain:

- **Goal** — one paragraph, the observable outcome.
- **Files & interfaces** — the specific files to change and the shapes (functions,
  types, endpoints) involved.
- **Approach** — the plan, referencing existing patterns to follow.
- **Out of scope** — what we are deliberately not doing.
- **Verification** — the exact end-to-end check that proves the feature works
  (the test command, the tests to write, or the manual check).

Then stop. Tell the user to review `SPEC.md`, edit it directly if needed, and start
a **fresh session** with `/implement` so implementation begins with clean context.
If the spec is clearly more than one session of work, recommend `/breakdown` first.
