---
name: tdd
description: Test-driven development discipline — red, green, refactor. Reach for this whenever writing new behavior or fixing a bug, so that every change is backed by a test that was seen to fail first.
---

# Test-driven development

The rule that makes tests trustworthy: **see the test fail before you make it pass.**
A test that has never failed proves nothing.

## The loop

1. **Red.** Write one small test describing the next slice of behavior. Run it.
   Confirm it fails, and that it fails *for the reason you expect* (not a typo or a
   missing import).

2. **Green.** Write the minimum code to make that test pass — nothing more. Run the
   test. Confirm it passes.

3. **Refactor.** With the test green as a safety net, clean up the code and the test.
   Re-run to confirm still green.

Repeat for the next slice.

## Practices

- One behavior per test. Small tests localize failures.
- Prefer running the **single** relevant test during the loop; run the full suite at
  checkpoints. (See the test commands in CLAUDE.md.)
- Match the project's existing test framework, file layout, and assertion style —
  read a neighboring test file first.
- Avoid heavy mocking. Test real behavior where you reasonably can.
- Cover the edge cases explicitly: empty, boundary, error, and unexpected input.
- Never delete or weaken a failing test just to get to green. If the test is wrong,
  fix the test deliberately and say why.
