---
name: implement
description: Implement a feature from a SPEC.md or an agreed plan using test-driven development, verifying continuously. Use once the approach is decided and it is time to write code.
disable-model-invocation: true
---

# Implement from a spec

Precondition: there is an agreed plan — ideally a `SPEC.md` from `/plan-feature`.
If there isn't one and the change is non-trivial, stop and run `/plan-feature` first.

## Steps

1. **Load the plan.** Read `SPEC.md` (or restate the agreed plan) and confirm the
   verification check it names. If no check is defined, define one now — you need a
   pass/fail signal.

2. **Work in small increments.** Take the smallest slice that produces observable
   behavior. Do not build the whole thing before running anything.

3. **TDD each slice** (see the `tdd` skill):
   - Write a failing test for the slice. Run it. Confirm it fails for the right reason.
   - Write the minimum code to pass. Run the test. Confirm green.
   - Refactor while green.

4. **Follow existing patterns.** Match the style, error handling, and structure the
   codebase already uses. Don't introduce new libraries unless the spec allows it.

5. **Verify as you go.** After each slice, run the relevant single test. After a
   group of changes, run the typecheck/build and lint from CLAUDE.md.

6. **Final verification.** Run the full check named in the spec and **show the
   output** — do not just assert it passed.

7. **Gate on the Definition of Done.** Walk the checklist in CLAUDE.md item by
   item and state the evidence for each. Any unchecked item means the work is
   not done — no exceptions.

8. **Review, then ship.** Run `/review-diff` for an independent check, address
   Critical/Major findings, then `/ship`.

Stay inside the spec's scope. If you discover the spec is wrong, stop and flag it
rather than silently expanding the work.
