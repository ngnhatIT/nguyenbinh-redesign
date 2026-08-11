---
name: handoff
description: Compact the current session into a HANDOFF.md so a fresh session or another agent can continue without re-deriving context. Use when context is getting full, when pausing a long task, or before handing work to someone else.
disable-model-invocation: true
---

# Hand off the session

When a task spans sittings or context is filling up, don't rely on the conversation
history surviving. Write the state down.

Write a `HANDOFF.md` containing exactly what the next session needs — no more:

- **Goal** — what we are trying to achieve (one paragraph).
- **Status** — what is done and verified, and what remains.
- **Current state of the code** — files changed so far, on which branch, and whether
  it is currently green or red.
- **Next steps** — the concrete next actions, in order.
- **Open questions / decisions** — anything unresolved that blocks progress.
- **How to verify** — the check command that proves the work.

Keep it factual and skimmable. Then tell the user they can `/clear` (or start a fresh
session) and point the next session at `HANDOFF.md` to continue with clean context.
