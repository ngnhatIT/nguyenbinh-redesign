---
name: breakdown
description: Split large, multi-session work into small dependency-ordered tickets, each independently verifiable. Use when a feature or migration is too big for one session or one PR.
disable-model-invocation: true
---

# Break down large work

Big work fails when attempted in one pass: context fills, verification comes too
late, and a single mistake poisons everything after it. Split it into
**tracer bullets** — thin end-to-end slices that each prove something works.

## Steps

1. **Start from a spec.** If there's no SPEC.md, run `/plan-feature` first.

2. **Slice vertically, not horizontally.** Each ticket should cut through the
   stack and produce observable behavior ("user can log in with Google, happy
   path only"), not a layer ("write all the models"). The first ticket is the
   thinnest possible end-to-end proof.

3. **Write TICKETS.md** with one entry per ticket:
   - **ID and title** — imperative, small enough for one session.
   - **Goal** — the observable behavior when done.
   - **Files touched** — best guess.
   - **Verification** — the exact check proving this ticket, runnable at THIS
     ticket's completion (not "at the end of the project").
   - **Blocked by** — ticket IDs that must land first.

4. **Sanity-check the graph.** No cycles; every ticket ≤ one session of work;
   anything bigger gets split again. Order so risk and unknowns land early.

5. **Execute one ticket per session.** For each: fresh session → `/implement`
   pointing at the ticket → Definition of Done → `/ship` → mark done in
   TICKETS.md. Use `/handoff` if a ticket unexpectedly spans sessions.

Re-plan when reality disagrees: if a ticket reveals the map is wrong, update
TICKETS.md before continuing — don't push through a stale plan.
