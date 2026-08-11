---
name: fix-bug
description: Diagnose and fix a bug by first reproducing it with a failing test, then fixing the root cause and confirming the test goes green. Use for any reported defect or failing behavior.
disable-model-invocation: true
---

# Fix a bug

Do not jump to a fix. A bug you can't reproduce is a bug you can't confirm fixed.

## Steps

1. **Capture the symptom.** Get the exact error, stack trace, and the steps or input
   that trigger it. Ask the user for the reproduction command if it isn't obvious.

2. **Reproduce with a failing test.** Write a test that fails *because of this bug*.
   Run it and confirm it fails for the right reason. This is your proof.
   - If a unit test can't reach it, write the smallest script or command that
     reproduces it deterministically.

3. **Find the root cause — hypothesis loop.** Do not change product code while
   diagnosing. Iterate:
   - State a falsifiable hypothesis: "X happens because Y".
   - Test it cheaply: read the code path, add temporary logging/instrumentation,
     or shrink the reproducing input.
   - Confirmed → proceed. Refuted → new hypothesis with what you learned.

   Delegate wide code reading to the `explorer` subagent to keep context clean.
   If three hypotheses in a row fail, step back and question an assumption
   ("is this even the code that runs?") instead of generating a fourth variant.

4. **Fix the cause, not the symptom.** Do not suppress the error, add a blanket
   try/catch, or special-case the one input. Fix why it happens.

5. **Confirm green and clean up.** Run the failing test from step 2 — it must now
   pass. Then run the surrounding tests to check you didn't break anything. Show
   the output. Remove every piece of temporary instrumentation added in step 3.

6. **Guard against regression.** Keep the test from step 2. It is now a permanent
   guard. Then `/ship`.
