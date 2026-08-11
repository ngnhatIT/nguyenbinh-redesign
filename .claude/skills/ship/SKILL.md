---
name: ship
description: Commit the current work with a descriptive message and open a pull request. Has side effects (writes git history, pushes) so it is only ever run when the user explicitly invokes it.
disable-model-invocation: true
---

# Ship it

Only run when the work is verified green and reviewed. This skill writes git history
and pushes, so never invoke it automatically.

## Steps

1. **Gate on the Definition of Done.** Walk the checklist in CLAUDE.md and state
   the evidence for each item. Any unchecked item → stop, do not ship.

2. **Pre-flight scan of the diff.** Run `git status`, `git diff`, and
   `git diff --staged`, then check the actual diff text for:
   - debug leftovers: `console.log`/`print`/`debugger`/`TODO` without a ticket
   - secrets: anything matching key/token/password/credential patterns
   - unrelated files or accidental formatting-only churn
   - files that should never be committed (`.env*`, local configs, build output)

   Anything found → fix first.

3. **Commit** on a feature branch (never commit straight to the default branch).
   Follow the commit style in CLAUDE.md (Conventional Commits by default) and
   explain *why*, not just *what*:
   - Subject: type + imperative summary (`feat: add Google OAuth callback handler`).
   - Body: the motivation and any non-obvious decisions.

4. **Push and open a PR** with `gh pr create`. The PR description should cover:
   - What changed and why.
   - How it was verified (the check that passed).
   - Any risks or follow-ups the reviewer should watch for.

5. **Report the PR URL** back to the user.

Follow the branch-naming and PR conventions in CLAUDE.md if they are set.
