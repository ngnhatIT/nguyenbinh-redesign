---
name: onboard
description: Understand a codebase and configure the scaffold for it — detect build/test/lint commands, fill CLAUDE.md placeholders, and write the CONTEXT.md domain vocabulary and architecture map. Run once per project, and re-run when architecture shifts.
disable-model-invocation: true
---

# Onboard to a codebase

Goal: after this skill runs, CLAUDE.md and CONTEXT.md describe *this* project
accurately, and every later session starts smarter.

## 1. Detect the stack

Read the manifest files (`package.json`, `pyproject.toml`, `go.mod`, `pom.xml`,
`Makefile`, CI config…) to find the **actual** commands for install, build/typecheck,
test (single and all), and lint. Run the harmless ones (`--version`, `--help`,
a single fast test) to confirm they work — do not guess.

## 2. Map the architecture

Use the `explorer` subagent for wide reading:

> Use the explorer subagent to map this codebase: entry points, core domain logic,
> data access, test layout, and the 5–10 domain terms used with specific meaning.

## 3. Interview the human

Use `AskUserQuestion` for what code can't tell you:

- What does the product do and for whom?
- Which terms have precise meanings a newcomer would misuse?
- Key decisions someone would re-litigate; gotchas that cost hours.
- Branch/PR conventions and anything unusual about the dev environment.

## 4. Write the files

- **CLAUDE.md** — replace the `Commands` and `Conventions` placeholders with
  verified values. Delete example lines. Keep it ruthlessly short.
- **CONTEXT.md** — fill all sections: what the project is, domain vocabulary
  table, architecture map (paths, not prose), key decisions & gotchas.
- **`.claude/settings.json`** — add the verified test/lint/build commands to the
  permission allowlist; suggest activating the PostToolUse lint hook and the
  Stop verification hook with the real commands.

## 5. Verify

Show the human a diff-style summary of what was written and confirm the commands
by running each once. Fix anything that fails before finishing.
