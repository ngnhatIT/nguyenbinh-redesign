---
name: review-diff
description: Adversarially review the current uncommitted diff in a fresh perspective before shipping. Checks correctness and spec-compliance, not style preferences. Use before committing anything non-trivial.
disable-model-invocation: true
---

# Review the diff

The engineer who wrote the code is biased toward it. This skill takes the opposite
stance: assume there is a problem and try to find it.

Delegate to the `reviewer` subagent so the review runs in a fresh context that sees
only the diff and the criteria — not the reasoning that produced the change:

> Use the reviewer subagent to review the current diff.

## Two axes

Review along two independent axes:

1. **Standards** — is the code correct and sound on its own terms?
   - Bugs, logic errors, off-by-one, unhandled nulls.
   - Race conditions, resource leaks, error handling.
   - Security: injection, secrets in code, unsafe input handling.
   - Consistency with existing patterns in the codebase.

2. **Spec** — does it do what was asked?
   - Every requirement in `SPEC.md` (or the agreed plan) is implemented.
   - The named edge cases have tests.
   - Nothing outside the task's scope changed.

## Reporting

Report only findings that affect **correctness or the stated requirements**. A
reviewer asked to find gaps will always find some; do not chase style nits or invent
defensive code for cases that can't happen.

Rank every finding by severity:

- **Critical** — wrong results, data loss, security hole, or crash on realistic
  input. Must fix before shipping.
- **Major** — a stated requirement is missing or an edge case named in the spec
  is unhandled/untested. Must fix before shipping.
- **Minor** — real but low-impact (unclear naming against CONTEXT.md, missing
  test for an unlikely path). Fix if cheap; otherwise note as a follow-up.

For each finding: file and line, what's wrong, and the concrete failure it causes
(input → wrong result).

If nothing real is found, say so plainly and proceed to `/ship`.
