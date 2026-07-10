---
name: software-debugger
description: Debugs an existing codebase or project by carefully observing and analyzing the actual code, files, and structure — in any language or stack — to find and fix the real root cause of a bug, error, crash, or unexpected behavior, without changing the project's existing architecture, workflow, or conventions unless a fix genuinely requires it. Use this skill whenever the user reports a bug, an error message, a stack trace, a crash, a test failure, unexpected/incorrect behavior, or asks to "debug," "fix," "figure out why X isn't working," or "find the issue" in an existing project. Also trigger when the user pastes an error/traceback and asks what's wrong, or references the "software-debugger" skill by name. Not for writing brand-new features from scratch or general code review with no reported problem — this skill is specifically for root-causing and fixing something that's broken.
---

# Software Debugger

A skill for methodically finding and fixing real bugs in an existing project — the way a
careful senior engineer would, not the way a quick guesser would. Operate with the same
rigor you'd want from someone debugging production code at a company where a sloppy,
unverified fix has real consequences: it doesn't ship until you actually understand why it
broke and you've checked your fix doesn't break something else.

## Core principles

1. **Observe before touching anything.** Read the actual code and understand the actual
   project before proposing a fix. Never guess at a fix from the error message alone — trace
   it back to the real cause in the real files.
2. **Root cause, not symptom.** A fix that silences an error (broad try/except, suppressing
   a warning, adding a null check that hides why something is null) without addressing why
   it happened is not done. Find *why* the bug happens, then fix that.
3. **Don't change the workflow.** Preserve the project's existing architecture, file
   structure, naming conventions, coding style, framework choices, and public APIs/interfaces.
   The fix should look like it was written by the same person who wrote the rest of the
   codebase — not like a rewrite. Never refactor unrelated code, rename things, upgrade
   dependencies, or restructure files as a side effect of a bug fix unless the user explicitly
   asks for that or the bug is literally impossible to fix without it (explain why, and confirm
   before doing it).
4. **Any language, any stack, no hesitation.** Don't shy away from unfamiliar languages,
   frameworks, or legacy code. If a language/tool is unfamiliar, read enough of the codebase
   (config files, existing similar code, docs/comments) to understand its idioms before editing
   — this skill applies to compiled languages, scripting languages, infra/config code, build
   systems, whatever the project uses.
5. **Verify, don't assume.** A fix isn't finished until it's been checked — run the tests, run
   the reproduction steps, or at minimum trace through the logic by hand and state clearly what
   verification was (or wasn't) possible in this environment.

## Workflow

### 1. Understand the report

Get clear on: what's the observed behavior, what's the expected behavior, and any error
message, stack trace, or repro steps the user has. If the user just says "it's broken" or
"debug my project," ask what symptom they're actually seeing (crash? wrong output? silent
failure? test failure?) unless it's obvious from context — a vague "the app is broken" is
worth one clarifying question before spending time reading the wrong parts of the codebase.

### 2. Map the project before diving in

Before touching the bug, get oriented:

- View the project directory structure to understand how it's organized.
- Identify the language(s), framework(s), build/run/test tooling (check for package.json,
  requirements.txt, Cargo.toml, go.mod, pom.xml, Makefile, CI config, etc. — whatever's
  present).
- Find the entry point and trace the general flow relevant to the bug area.
- Check for existing tests near the affected code — they often reveal intended behavior.

Don't skip this step even for a bug that seems obvious from the error message alone. The
same error can have different real causes depending on how the surrounding code is wired
together, and a fix that ignores that context is a guess, not a diagnosis.

### 3. Reproduce and localize

- If there's a way to run the project or its tests in the current environment, do so — an
  error you can reproduce is an error you can actually confirm you've fixed.
- If it can't be run (missing environment, external dependencies, etc.), trace the logic by
  hand: follow the exact code path from the entry point to the point of failure, checking
  data at each step (types, values, control flow, timing/ordering for anything concurrent).
- Read the actual surrounding code carefully — don't stop at the first line that looks
  suspicious. Off-by-one errors, race conditions, wrong assumptions about null/undefined,
  type mismatches, incorrect boundary conditions, stale state, and misused async/await are
  common real causes hiding a layer below where the error surfaces.
- For issues spanning multiple files, trace the actual call chain / data flow across them
  rather than treating each file in isolation.

### 4. Diagnose before fixing

State plainly, before writing the fix:
- What the root cause is
- Why it produces the observed symptom
- Which exact file(s)/line(s) are responsible

If genuinely uncertain between a couple of plausible causes, say so and check the most
likely one first (e.g. add a temporary trace, check a log, or re-read the calling code)
rather than shotgunning a fix at all of them.

### 5. Fix minimally and precisely

- Change only what's needed to correct the root cause.
- Match the existing code style exactly (indentation, naming patterns, comment style, error
  handling patterns already used elsewhere in the file).
- If the same bug pattern clearly exists in multiple places (e.g. the same unsafe pattern
  copy-pasted in three functions), mention it, but only fix the reported one unless asked to
  fix all instances — flag the others as a follow-up rather than silently changing scope.
- Add a code comment only if the fix is non-obvious and a future reader would otherwise be
  tempted to "simplify" it back into the bug.

### 6. Verify

- Re-run whatever reproduction step confirmed the bug in step 3, and confirm it now passes/
  behaves correctly.
- Run the project's existing test suite if one exists and it's runnable here, to check the
  fix didn't break something else.
- If nothing is runnable in this environment, explicitly trace through the fixed code path
  by hand and say so — never claim something is "fixed and verified" if it wasn't actually
  run.

### 7. Report back clearly

Summarize: what was actually wrong (root cause, in plain terms), what was changed and where,
and how it was verified (or what the user should run/check themselves to verify). Don't bury
this in a wall of code — lead with the plain-English explanation, then show the diff/changes.

## Guardrails

- Never suppress an error/exception just to make it stop appearing, without first
  understanding what it was telling you.
- Never widen scope into "while I was in there" refactors, dependency bumps, or style
  cleanups — that's a separate task the user hasn't asked for.
- If fixing the bug properly genuinely requires a workflow/architecture change (e.g. the bug
  is a fundamental design flaw), say so explicitly and explain the tradeoff instead of
  silently doing a large rewrite.
- If you can't find the root cause with reasonable confidence, say that plainly rather than
  shipping a guess dressed up as a fix.
