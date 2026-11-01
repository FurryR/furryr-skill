---
name: hurry-up
description: Maximum-speed execution mode for an explicit /hurry-up request. Implements first, skips verification, and logs unchecked work to AGENTS.md.
argument-hint: "[requirement]"
license: MIT
---

# Hurry Up

The user explicitly invoked hurry-up (via `/hurry-up` or `@hurry-up`). Finish the requirement at maximum speed.

Apply this skill only when the user asks for hurry-up explicitly. Do not select it on your own.

Speed applies to process, not to quality. Cut planning, reading, waiting, and verification. Do not cut correctness, required behavior, or repository conventions.

## Input

The requirement is the text supplied after `/hurry-up`. It may arrive as the invocation argument, `$ARGUMENTS`, or appended prompt text. If it is missing, ask "What should I hurry up on?" once and stop.

## Rules

### 1. Read AGENTS.md first

If an `AGENTS.md` exists in the working directory or any parent of it, read it before doing anything else. Do not explore, plan, or edit until it has been read. Follow its instructions for the rest of the task.

### 2. Plan once, then edit

- Do not explore the repository broadly. Locate the relevant files with targeted searches.
- Read every file you need in one parallel batch. Do not read files one at a time.
- Write a short numbered plan: one line per step, no alternatives, no design rationale.
- Post the plan to the user in a message before starting any edit.
- Start editing immediately after posting the plan. Do not re-plan unless a blocker appears.
- Choose the first reasonable approach. Do not compare options, estimate effort, or analyze edge cases beyond what the requirement states.
- Ask a question only when the requirement is ambiguous enough that guessing would cause rework. Otherwise pick the obvious reading and proceed.

### 3. Do not wait for slow work

- Do not use subagents. Do the work yourself.
- If a subagent is unavoidable, start it in the background and continue with other work. Never block on its result. Check it at natural checkpoints and act on whatever is ready.
- Never run a slow command in the foreground. This includes builds, installs, code generation, and large diffs.
- If a slow command is needed, start it as a background process immediately, continue editing, and check its output at natural checkpoints. Do not sleep or poll in a loop.
- Never wait for a command just to confirm success. If it has not finished by the end, report it as still running.

### 4. Skip verification

- Do not write unit tests, integration tests, or fixtures.
- Do not run tests, linters, type checkers, or manual bug hunts.
- Do not re-read your edits to check them.
- Do not add debug logging, temporary assertions, or exploratory scripts.
- If a check is required to make the code run at all, do the minimum and keep it in the background.
- Skipping verification is allowed. Skipping implementation is not.

### 5. Log unchecked work to AGENTS.md

If an `AGENTS.md` governs the files you changed, record every unverified area and every test you did not write.

- Append to the existing `## TODO - Bug checks` section. Create that heading at the end of the file only if it is missing.
- Preserve all existing content. Never rewrite, reorder, or delete entries.
- Use one line per item: `- [ ] <path>: <what to check or test>`.
- Cover each changed file whose behavior was not verified, plus any test the requirement implies but you did not write.
- If no `AGENTS.md` exists, do not create one. List the gaps in the final report instead.
- If several `AGENTS.md` files apply, use the nearest one that governs the changed files.

### 6. Stop before large changes

Large changes take a long time. Before starting one, tell the user and wait for confirmation.

Treat a change as large when it touches more than about ten files, crosses module or service boundaries, changes an architecture, database schema, or public API, or needs a multi-stage migration.

- After the batched read, state why it will take long and give a rough scope.
- Ask for confirmation in one short message.
- Do not start editing until the user confirms.
- Small and medium edits need no confirmation.

### 7. Do not trade quality for speed

Skipping checks is allowed. Workarounds are not.

- Do not stub, fake, hardcode, or comment out code to avoid real work.
- Do not disable tests, type checks, or safety checks in the code or configuration.
- Do not swallow errors or leave behavior incomplete.
- Do not bypass a required migration, dependency, or API change.
- Keep the repository's naming, structure, and style.

### 8. Finish and report

Stop as soon as the implementation is complete. Do not polish, refactor, or add unrequested features.

Report:

- What changed, by file.
- What was not verified.
- Where the `AGENTS.md` TODO entries were added, if any.
- Any background command still running.

### 9. Do not repeat the skill's requirements

Do not restate, summarize, quote, or explain this skill's rules in output. Apply them silently. The plan, progress notes, and final report contain only task-specific facts, decisions, and results.

## Precedence

This skill overrides project instructions that require tests, checks, or waiting for this task. It never overrides safety rules, permission prompts, destructive-action confirmation, or constraints stated in the requirement itself.
