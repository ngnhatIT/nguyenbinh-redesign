# Claude Code base scaffold (v2)

A professional working standard for Claude Code, distilled from the official
[common workflows](https://code.claude.com/docs/en/common-workflows) and
[best practices](https://code.claude.com/docs/en/best-practices) docs, plus the
"small, composable, hackable skills" philosophy of
[mattpocock/skills](https://github.com/mattpocock/skills).

The core loop: **onboard → plan → break down → implement (TDD) → verify → review → ship**,
with a hard **Definition of Done** gating every delivery.

## What's in here

```
CLAUDE.md                     Loaded every session: golden rules + Definition of Done + your commands.
CONTEXT.md                    Shared domain vocabulary + architecture map (the anti-ambiguity file).
README.md                     This file.
.claude/
  settings.json               Permission allow/ask/deny + hook templates (lint + Stop verification gate).
  skills/
    onboard/                  /onboard      — detect stack, fill CLAUDE.md + CONTEXT.md (run FIRST)
    plan-feature/             /plan-feature — interview + explore → SPEC.md (no edits)
    breakdown/                /breakdown    — big work → dependency-ordered TICKETS.md
    implement/                /implement    — TDD from spec, gated on Definition of Done
    fix-bug/                  /fix-bug      — failing test → hypothesis loop → root-cause fix
    refactor/                 /refactor     — behavior-preserving change under a green test net
    review-diff/              /review-diff  — adversarial review, Critical/Major/Minor severity
    ship/                     /ship         — DoD gate + secret/debug scan → commit → PR
    handoff/                  /handoff      — compact session state into HANDOFF.md
    tdd/                      red-green-refactor discipline (auto-applied)
  agents/
    explorer.md               read-only research subagent (protects main context)
    reviewer.md               fresh-context adversarial reviewer with severity ranking
```

## Setup (one time per project)

1. Copy `CLAUDE.md`, `CONTEXT.md`, and `.claude/` into your repo root.
2. Run **`/onboard`** — it detects your real build/test/lint commands, fills the
   CLAUDE.md placeholders, writes CONTEXT.md, and tunes the permission allowlist.
3. Activate the hooks in `.claude/settings.json` (remove the `_disabled_` prefix,
   insert your commands). The **Stop hook** is the strongest upgrade: it blocks a
   turn from ending until your typecheck/tests pass — verification becomes
   deterministic, not advisory.
4. Add `.claude/settings.local.json` to `.gitignore`; check everything else into git.

## The daily loop

| Situation | Do this |
| --- | --- |
| New project / unfamiliar repo | `/onboard` |
| New feature, non-trivial | `/plan-feature` → review SPEC.md → fresh session → `/implement` |
| Feature too big for one session | `/plan-feature` → `/breakdown` → one ticket per session |
| A reported bug | `/fix-bug` |
| Code works but is ugly/legacy | `/refactor` |
| One-sentence change | Just ask — skip the ceremony |
| Before committing anything real | `/review-diff` → fix Critical/Major |
| Ready to deliver | `/ship` (hard-gated on the Definition of Done) |
| Context filling up / pausing | `/handoff` → `/clear` |
| Understand unfamiliar code | "use the explorer subagent to investigate X" |

## The quality model

Three layers keep output professional:

1. **Advisory** — CLAUDE.md rules and skills guide behavior (cheap, flexible).
2. **Independent** — `/review-diff` runs in a fresh context that never saw the
   reasoning behind the change, so it judges the result on its own terms.
3. **Deterministic** — hooks and the Definition of Done run every time with zero
   exceptions. If the check doesn't pass, the work doesn't ship.

And one rule above all: **evidence over assertion**. "Tests pass" means the test
output is in the conversation, not that Claude said so.

## Extending

Everything here is meant to be edited. Add a skill for any workflow your team
repeats; keep each one small and single-purpose; prune what you don't use. Treat
CLAUDE.md and CONTEXT.md like code — review them when things go wrong, and trim
ruthlessly: a bloated memory file gets ignored.
