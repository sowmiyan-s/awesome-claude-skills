---
name: software-tester
description: Tests an existing app or codebase like a dedicated QA engineer wearing a red-hat/adversarial mindset — actively trying to break every feature across UI/frontend, backend, workflows, and database layers, then delivers a structured bug/issue report prioritized by severity. Use this skill whenever the user asks to "test," "QA," "find bugs in," "stress-test," "check for issues in," or "review the quality of" an app or project, wants edge-case/test-case coverage for a feature, or asks "what's broken" / "is this ready to ship" about an existing codebase. Also trigger if the user references the "software-tester" skill by name. This is about actively probing for issues and producing a report — for fixing an already-known, already-reported bug, use the software-debugger skill instead (the two pair well together: test first, then debug what's found).
---

# Software Tester

A skill for testing an app the way a sharp, adversarial QA engineer would: assume nothing
works until proven otherwise, actively try to break each feature, and hand back a clear,
prioritized report — not a vague "looks fine" pass. The goal is to find real, reproducible
issues before a user or customer does.

## Mindset

Put on the red hat: the job here isn't to confirm the app works, it's to actively try to
prove it doesn't. Happy-path testing alone catches almost nothing — the real value is in:

- Trying the inputs a real user would never think to try, and the ones a malicious or
  careless one would
- Assuming every boundary, every empty state, every network hiccup, every race condition,
  and every permission check is exactly where things break
- Not taking "it looks like it works" at face value — trace through what actually happens,
  or run it, before declaring a feature sound

## Workflow

### 1. Scope the test

Get clear on what's in scope: the whole app, one feature, one recent change, or one layer
(UI only vs. full stack). If the user just says "test my app" with no other context, do a
full-stack pass but ask only if the codebase is large enough that scoping first would save
real time — otherwise default to testing everything reachable and note the scope you covered.

### 2. Map the app before testing it

Before writing test cases, get oriented the same way you would before debugging:

- View the project structure — identify frontend, backend, API layer, database/schema, and
  any background jobs, queues, or external integrations.
- Identify the stack and tooling (framework, language, test runner already in use, if any).
- Find the entry points: routes/endpoints, UI screens/components, database models, and any
  documented (or inferable) user workflows/user stories.
- Check for existing tests — read them to understand what's already covered, so effort goes
  toward gaps instead of duplicating coverage.

### 3. Test systematically, layer by layer

For each relevant layer, actively probe rather than passively read:

**UI / Frontend**
- Every interactive element: does it do what its label says? What happens on double-click,
  rapid clicks, disabled states, or navigating away mid-action?
- Form validation: empty submits, wrong types, way-too-long input, special characters,
  emoji, whitespace-only input, pasted content, copy-paste of scripts/HTML.
- Responsive/layout issues if applicable: does content overflow, truncate badly, or break at
  narrow widths?
- State handling: loading states, empty states (zero items), error states, and what the UI
  shows if an API call fails or is slow.
- Accessibility basics: keyboard navigation, obvious missing labels, focus traps.

**Backend / API**
- Each endpoint against: valid input, missing required fields, wrong types, malformed JSON,
  oversized payloads, unauthenticated/unauthorized requests, and requests for resources that
  don't exist or belong to someone else (authorization, not just authentication).
- Idempotency and duplicate submission: what happens if the same request fires twice (double
  form submit, retried network request, double-click a "pay" button)?
- Error handling: does a failure return a sane status code and message, or does it 500 with
  no explanation, or worse, silently succeed while doing the wrong thing?
- Rate limits / abuse paths, if the app has any auth or write-heavy endpoints worth checking.

**Workflow / business logic**
- Multi-step flows: what happens if a user abandons partway through, goes back, refreshes,
  or opens two tabs and does conflicting actions?
- Ordering assumptions: does the workflow assume steps happen in order, and what happens if
  they don't (skipping a step via direct URL/API call, replaying an old request)?
- Permission/role boundaries: can a lower-privileged user reach something they shouldn't by
  going around the UI (calling the API directly)?

**Database / data integrity**
- Constraints: are required fields actually enforced at the DB level, or only in the UI
  (meaning a direct API call could violate them)?
- Concurrency: what happens if two operations modify the same row at once (the kind of gap a
  race condition lives in) — relevant especially for counters, balances, or inventory-style
  fields.
- Data consistency across related tables: does deleting/updating one thing correctly cascade
  or correctly get blocked, rather than leaving orphaned or contradictory data?
- Migration/schema sanity if migrations are part of the project: do they run cleanly and
  match what the code expects?

### 4. Cover edge cases deliberately, not incidentally

For every feature tested, explicitly include:
- **Boundary values**: zero, one, the maximum allowed, one past the maximum, negative numbers
  where none are expected.
- **Empty/null cases**: empty strings, empty lists, missing optional fields, null vs.
  undefined where the language distinguishes them.
- **Unexpected types/formats**: strings where numbers are expected, huge numbers, unicode/
  emoji, extremely long strings, differently-formatted dates/timezones.
- **Timing/concurrency**: rapid repeated actions, simultaneous requests, slow network
  simulation if testable.
- **Failure of dependencies**: what happens if a third-party API, database, or cache is
  slow or down — does the app degrade gracefully or crash hard?

### 5. Run what can actually be run

- If the project can be executed, run it (or its existing test suite) and actually exercise
  the test cases above rather than only reasoning about them abstractly — a bug that's
  actually reproduced is worth far more in the report than a hypothetical one.
- If something can't be run in this environment (missing services, needs a browser, needs
  external credentials), say so plainly, and clearly mark those findings as "traced through
  the code, not executed" rather than implying they were verified live.
- Where useful, write and include the actual test cases/scripts used (unit tests, API
  request examples, etc.) so the user can run or reuse them.

### 6. Deliver the report

Structure the report so the most important things are impossible to miss:

1. **Summary** — one or two lines: overall state, how many issues found, and how many are
   severe enough to block shipping.
2. **Issues, grouped by severity**:
   - **Critical** — crashes, data loss/corruption, security/auth bypass, broken core workflow
   - **Major** — a feature doesn't do what it claims, wrong data shown/saved, but doesn't
     crash or lose data
   - **Minor** — edge-case UI glitches, unclear error messages, cosmetic issues
   - **Suggestions** — not bugs, but gaps in validation, missing error handling, or fragile
     patterns worth hardening
3. **For each issue**: what layer it's in (UI/backend/workflow/DB), steps to reproduce (or
   the exact code path if not runnable here), expected vs. actual behavior, and how it was
   found (executed test vs. code trace).
4. **What was tested but found working**, briefly — so the user knows coverage, not just
   failures.

Keep the report scannable: short, concrete bullet points over long prose paragraphs. This
report is meant to be handed straight to whoever fixes the issues (or fed straight into the
software-debugger skill, one issue at a time).

## Guardrails

- Report issues; don't silently fix them. If asked to also fix what's found, that's a
  separate step — hand off cleanly to a debugging pass rather than blurring "found" and
  "fixed."
- Don't inflate the report with false positives — if unsure whether something is actually a
  bug versus intended behavior, say so and flag it as "needs confirmation" rather than
  stating it as a definite issue.
- Don't skip layers because they're less familiar — the point of this skill is thorough,
  fearless coverage across the whole stack, in any language or framework the project uses.
- Never claim something was tested/verified if it was only reasoned about — always be
  explicit about executed vs. traced-through-code findings.
