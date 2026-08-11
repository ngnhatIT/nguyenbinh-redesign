# Project memory

Loaded at the start of every session. Keep it short — rule of thumb for every
line: *"Would removing this cause Claude to make a mistake?"* If not, delete it.

Domain vocabulary and architecture map: @CONTEXT.md

<!-- Fill in Commands/Conventions for YOUR project (or run /onboard to auto-fill). -->

## Commands

No build/test/lint toolchain — this is hand-written static HTML.

- Preview: open the target `.html` file directly in a browser (no server needed).
- Deploy: push to `main` → GitHub Pages serves it (`.nojekyll` bypasses Jekyll).
- Verify a change: reload the file in the browser and check visually.

## Conventions

- Each page is a **single self-contained file**: one inline `<style>`, inline
  `<script>`, no local `.css`/`.js`. Keep it that way — do not extract shared files.
- Design iterations are new numbered files (`v8.html`, …); `index.html` and every
  `v*.html` are live pages. Don't overwrite an existing version to iterate — copy
  it to the next number. (See CONTEXT.md.)
- Shared assets live in `img/`. Content is Vietnamese (`lang="vi"`).
- Commit style: Conventional Commits (`feat:`, `fix:`, `refactor:`, `docs:`, `chore:`)

---

# Golden rules (workflow)

1. **Verify with evidence.** Nothing is done until a pass/fail check has run —
   tests, build, lint, screenshot — and the output is shown, not asserted.
   If you can't verify it, don't ship it.

2. **Explore → plan → code → commit.** For anything beyond a one-sentence diff,
   agree a plan before editing (`/plan-feature`). Skip ceremony only for trivial
   changes.

3. **TDD by default.** New behavior and bug fixes start with a failing test that
   is seen to fail (`tdd` skill).

4. **Root causes, not symptoms.** Never suppress an error or special-case one
   input to hide a bug.

5. **Protect context.** Wide exploration goes to the `explorer` subagent;
   unrelated tasks start with `/clear`; long tasks checkpoint with `/handoff`.

6. **Match existing patterns.** Find how the codebase already does the similar
   thing and follow that shape. Use the vocabulary in CONTEXT.md.

7. **Small, reversible steps.** One change → verify → next. Commit at green
   checkpoints.

8. **Stay in scope.** If the spec turns out wrong mid-implementation, stop and
   flag it — don't silently expand the work.

# Definition of Done

Before `/ship`, ALL of these must hold (this is the gate, not a suggestion):

- [ ] Change viewed in a browser — layout/behavior confirmed (no build/test here)
- [ ] Page still renders correctly at mobile and desktop widths
- [ ] `/review-diff` ran; Critical/Major findings addressed
- [ ] No leftover debug code, commented-out blocks, TODOs without tickets
- [ ] No secrets, keys, or credentials anywhere in the diff
- [ ] Docs/CONTEXT.md updated if public interfaces or vocabulary changed

# Skills

| Command | Purpose |
| --- | --- |
| `/onboard` | Understand a codebase → fill CLAUDE.md commands + write CONTEXT.md |
| `/plan-feature` | Interview + explore → self-contained SPEC.md (no edits) |
| `/breakdown` | Split large work into small, dependency-ordered tickets |
| `/implement` | Build from spec with TDD, gate on Definition of Done |
| `/fix-bug` | Reproduce with failing test → root-cause fix → green |
| `/refactor` | Behavior-preserving improvement under a green test net |
| `/review-diff` | Adversarial review with severity ranking |
| `/ship` | Pre-flight checks → commit → PR |
| `/handoff` | Compact session state into HANDOFF.md |
| `tdd` | Red-green-refactor discipline (model-invoked) |

# Subagents

- `explorer` — read-only research in its own context
- `reviewer` — fresh-context adversarial diff reviewer
