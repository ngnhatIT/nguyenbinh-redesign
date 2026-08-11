---
name: refactor
description: Improve code structure without changing behavior, under a green test net. Use for modernizing legacy code, extracting duplication, simplifying complex modules, or paying down design debt.
disable-model-invocation: true
---

# Refactor safely

Refactoring changes structure, never behavior. The test suite is the definition
of "behavior" — so the net must exist *before* the first change.

## Steps

1. **Scope it.** Name the target (file/module), the smell (duplication, deep
   nesting, god object, deprecated API…), and the desired end shape. If the scope
   is "the whole codebase", run `/breakdown` first.

2. **Establish the net.** Run the existing tests for the target and confirm green.
   If coverage is thin, **write characterization tests first** — tests that pin the
   current behavior, including its quirks. No net, no refactor.

3. **Refactor in small, named moves.** One move at a time: extract function,
   inline variable, rename to CONTEXT.md vocabulary, replace conditional with
   polymorphism… After **each** move, run the tests. Green → next move;
   red → undo the move, don't debug forward.

4. **Keep behavior identical.** If you find an actual bug mid-refactor, stop —
   note it, finish the refactor, then fix the bug separately via `/fix-bug`
   (one concern per commit).

5. **Verify the whole.** Full test run + typecheck + lint. Confirm the public
   interface is unchanged (or that all callers were updated).

6. **Commit separately.** Refactor commits contain no behavior change
   (`refactor: ...`). Then `/ship`.
