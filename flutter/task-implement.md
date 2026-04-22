# task-implement

Executes the approved implementation plan for an active task.

## Usage

```
task-implement <task-filename>
```

The task file must be located in `specs/tasks/active/`.

---

## Objective

Mechanically execute each step of the approved plan, verify the results of each step before proceeding to the next, and close the task with a Summary documenting the work done.

---

## Preconditions

Verify that the task has:
- Status `active`
- **Implementation Plan** section filled in and not empty
- The plan must have been approved (the section exists and contains numbered steps)

If any of these conditions are missing, stop and ask the user to run `task-plan` first.

---

## Phase 1 — Rereading the Plan

Reread:
1. The complete task file (Context, Goal, Acceptance Criteria, Decisions, Implementation Plan)
2. The specs files referenced in the Decisions, if relevant to execution

Do not reinterpret the architectural decisions: they were made in `task-activate`. If a conflict arises between the plan and the actual codebase, stop and notify the user before proceeding.

---

## Phase 2 — Executing the Steps

Execute each step in the order defined in the plan. For each step:

1. Announce the current step: `Executing step N: {description}`
2. Perform the action (CREATE / MODIFY / DELETE / RUN / VERIFY)
3. Verify the result before moving on to the next step
4. Immediately report any deviation from the plan

### Execution Rules

**For step CREATE**
- Create the file exactly at the specified path
- Use the project's naming conventions (see `flutter-architecture.md`)
- Do not add code not covered by the plan without reporting it

**For step MODIFY**
- Only modify the parts indicated in the plan
- Do not reformat the entire file unless necessary
- Preserve existing comments unless explicitly removed

**For step RUN**
- Run the command and display the output
- If the command fails, do not continue: analyze the error and present the user with options

**For step VERIFY**
- For commands (`flutter test`, `flutter analyze`): run and display output
- For manual checks: describe exactly what to check and wait for user confirmation

### Error Handling

If a step produces an error:

1. Display the complete error
2. Analyze the cause (dependency conflict, type error, missing import, etc.)
3. Propose a solution that respects the Decisions already made
4. Ask the user for confirmation before applying it
5. If the solution requires a change to the plan, update the **Implementation Plan** section in the task file

Do not attempt silent fixes: any deviation from the approved plan must be explicit.

---

## Phase 3 — Acceptance Criteria Checklist

After completing all steps, verify each acceptance criterion defined in the task:

```
Acceptance Criteria:
[ ] {criterion 1} — {PASS / FAIL / MANUAL}
[ ] {criterion 2} — {PASS / FAIL / MANUAL}
...
```

For automatically verifiable criteria (test, analyze), execute and report the result.

For criteria requiring manual verification, describe how to verify them and wait for user confirmation.

If one or more criteria fail, do not close the task: analyze and correct them before proceeding.

---

## Phase 4 — Compile the Summary

Fill in the **Summary** section of the task with:

```markdown
## Summary

Implementation completed on {date}.

### What was done
{3-7 line prose description of what was accomplished}

### Files created
- `{path}` — {short description}

### Files changed
- `{path}` — {description of change}

### Dependencies added
- `{package}: {version}` — {reason}
(or "None")

### Post-implementation notes
{any observations raised during implementation, identified technical debt, suggestions for future tasks}

### Acceptance Criteria
- [x] {criterion 1}
- [x] {criterion 2}
```

---

## Step 5 — Closing the task

1. Change **Status** from `active` to `done`
2. Move the file from `specs/tasks/active/` to `specs/tasks/done/`
3. Update `specs/flutter/flutter-widgets.md` if new reusable widgets have been created.
4. Update `specs/flutter/flutter-data-models.md` if data models have been created or modified.
5. Update `specs/flutter/flutter-routing.md` if routes have been added.

---

## Phase 6 — Final Communication

```
Task Completed: {title}

Files Created: {N}
Files Modified: {N}
Tests Added: {N}
Acceptance Criteria: {N}/{N} Verified

Task Stored in: specs/tasks/done/{filename}.md
```

If opportunities for future tasks (technical debt, enhancements, missing tests) arose during implementation, list them as suggestions with the appropriate task type.