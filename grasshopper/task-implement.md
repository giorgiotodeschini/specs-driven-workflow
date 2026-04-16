---
description: Implement a Grasshopper task
agent: build
---

If $ARGUMENTS is empty,
read the @specs/tasks/active/ directory, find all .md files where
## Implementation Plan section is present and not empty AND
## Summary section is missing or empty,
and report: "No task specified. Tasks ready for implementation:
[list of filenames without extension]
Usage: task-implement <task-name>"
Then stop.

If the file @specs/tasks/active/$ARGUMENTS.md does not exist,
stop immediately and report: "Task not found: specs/tasks/active/$ARGUMENTS.md"

Read @specs/tasks/active/$ARGUMENTS.md.

If the ## Decisions section is missing or empty,
stop immediately and report: "Task has not been activated. Run task-activate first."

If the ## Implementation Plan section is missing or empty,
stop immediately and report: "Task has no implementation plan. Run task-plan first."

Before proceeding, ensure you have read the project configuration file
and all documents it references.

The implementation plan has been approved. Do not ask clarifying questions.

If you encounter an unexpected situation that requires a decision not covered
by the plan, stop and ask me before proceeding.
Do not deviate from the approved plan without explicit approval.

Then follow the workflow that matches ## Type:

---

## BRANCH: new component / modify component / bugfix

1. Implement the plan step by step, following the order defined
   in ## Implementation Plan.
2. After each step, briefly report what was done before moving to the next.
3. When implementation is complete:
   - Update the ## Status section to "done"
   - Move the file to specs/tasks/done/
   - Write ## Summary with this structure:

## Summary
**Implemented:** [description of what was done]
**Files modified:** [list of files created or modified]
**Docs updated:** [grasshopper-components.md ✓ / data-model.md ✓ / none]
**Deviations:** [any deviations from the plan, or "none"]

---

## BRANCH: spike

1. Execute the research or analysis defined in ## Implementation Plan.
2. After each research step, briefly report what you found before moving
   to the next.
3. If you encounter an unexpected finding that changes the scope of the
   research, stop and ask me before proceeding.
4. When research is complete:
   - Write the conclusions under a ## Findings section structured as:
     **Conclusion:** [the main answer or decision reached]
     **Evidence:** [what you found that supports it]
     **Open questions:** [anything that remains unresolved]
   - If the spike identified follow-up work, list the suggested tasks
     under a ## Suggested Tasks section, each with a one-line description.
   - Update the ## Status section to "done"
   - Move the file to specs/tasks/done/
   - Write ## Summary with this structure:

## Summary
**Implemented:** [description of research conducted]
**Docs updated:** [grasshopper-components.md ✓ / data-model.md ✓ / none]
**Deviations:** [any deviations from the plan, or "none"]

   - Ask me: "Should I create the suggested tasks in the backlog?"
   - If I confirm, create each suggested task in specs/tasks/backlog/
     using the standard template, prefilling Context and Goal
     from the spike findings.

---

## BRANCH: refactor

1. Implement the plan step by step, following the order defined
   in ## Implementation Plan.
2. After each step, briefly report what was done before moving to the next.
3. When implementation is complete:
   - Update the ## Status section to "done"
   - Move the file to specs/tasks/done/
   - Write ## Summary with this structure:

## Summary
**Implemented:** [description of what was refactored]
**Files modified:** [list of files modified]
**Docs updated:** [grasshopper-components.md ✓ / data-model.md ✓ / none]
**Deviations:** [any deviations from the plan, or "none"]
