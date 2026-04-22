# task-new

Conduct a guided interview to create a new task in the Flutter project backlog.

## Objective

Gather all the information needed to create a well-formed task file in `specs/tasks/backlog/`. The task must be detailed enough to allow `task-activate` to analyze the codebase without underlying ambiguity.

---

## Phase 1 — Type and Title

Ask the user:

1. **Short Title** of the task (will be used for the file name, in kebab-case)
2. **Type** — presents the options:

- `feature` — new user functionality
- `bugfix` — fix incorrect behavior
- `refactor` — structural improvement without behavior change
- `spike` — technical exploration or proof of concept
- `widget` — create or modify a reusable widget
- `screen` — new screen with routing and state management
- `state` — introduce or restructure a Riverpod state slice
- `platform` — platform-specific code (MethodChannel, plugins, permissions)
- `theme` — design token, ThemeData, dark mode
- `test` — add or complete tests (unit, widget, integration, golden)
- `gen` — setup or modify code generation (frozen, json_serializable, riverpod_generator)

---

## Phase 2 — Common Questions (all types)

3. **Goal**: What is this task supposed to do or solve? (1-3 sentences)
4. **Context**: Why is it needed now? Is there a ticket, a user request, an observed problem?
5. **Scope**: What is explicitly INCLUDED and what is EXCLUDED?
6. **Acceptance Criteria**: How do you verify that the task is completed? (Ask for at least 2-3 concrete and verifiable criteria)
7. **Dependencies**: Are there other active or backlog tasks on which this depends? Are they upstream `gen` or `state` tasks?
8. **References**: Are there tickets, PRs, screenshots, design documents, URLs to attach?

---

## Phase 3 — Type-Specific Questions

Ask additional questions based on the type selected.

### `widget`
- Is there already a similar widget in the project? If so, where?
- Is the widget purely presentational (no global state) or does it interact with Riverpod?
- What are the expected public parameters?
- Is a golden test needed?

### `screen`
- What is the expected go_router navigation path?
- Which screen can I get to and where can I go?
- Which Riverpod providers are required?
- Is the screen only accessible with authentication?

### `state`
- Which state slice is being introduced or modified?
- What is the trigger (new feature, consistency bug, performance)?
- Are there any side effects (API calls, local persistence, notifications)?
- Which screens or widgets consume this state?

### `platform`
- Which platform is involved: Android, iOS, or both?
- Is there already a suitable pub.dev plugin, or do I need to write native code?
- Are there any permissions I need to ask the user for?
- What is the minimum OS version to support?

### `theme`
- Does it affect existing tokens (edit) or new tokens?
- Does it impact dark mode?
- Is it coordinated with an external design task or Figma?

### `test`
- What existing code needs to be covered?
- Predominant test type: unit, widget, integration, golden?
- Is there already a partial suite to complete?

### `gen`
- Which generator is involved: frozen, json_serializable, riverpod_generator, or other?
- Are there any backlogged/active tasks that depend on the generated files (and therefore blocked)?
- Does `build.yaml` need to be updated?

### `bugfix`
- How do I reproduce the bug? (specific steps)
- Expected behavior vs. actual behavior
- Is the bug tied to a specific app state or platform?

### `refactor`
- What is the current structural problem?
- Which files/classes are affected?
- Does the refactor change public widget or provider APIs?

### `spike`
- What is the technical question to be answered?
- What is the expected output (document, prototype, decision)?
- When do we need the answer?

---

## Step 4 — File Generation

After collecting all the answers, generate the task file with this template.

**File name**: `specs/tasks/backlog/YYYY-MM-DD-{title-kebab-case}.md`
Use today's date.

```markdown
# {Title}

## Status
backlog

## Type
{type}

## Dependencies
{task list or "None"}

## Context
{context gathered in the interview}

## Goal
{goal gathered in the interview}

## Scope

### Included
{scope included}

### Excluded
{scope excluded}

## Acceptance Criteria
{numbered list of criteria}

## References
{reference list, or "None"}

<!-- Sections filled by task-activate -->

## Notes

## Decisions

<!-- Sections filled by task-plan -->

## Implementation Plan

<!-- Filled by task-implement -->

## Summary
```

---

## Step 5 — Confirmation

Show the contents of the generated file The user is prompted and asked for confirmation before saving. If there are any corrections, apply them and display the result again.

After saving, provide the path to the created file and suggest the next command: `task-activate {file-name}`.