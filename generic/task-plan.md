---
description: Generate the implementation plan of the task
agent: build
---

If $ARGUMENTS is empty,
read the @specs/tasks/active/ directory, find all .md files where
## Decisions section is present and not empty AND
## Implementation Plan section is missing or empty,
and report: "No task specified. Tasks ready for planning:
[list of filenames without extension]
Usage: task-plan <task-name>"
Then stop.

If the file @specs/tasks/active/$ARGUMENTS.md does not exist,
stop immediately and report: "Task not found: specs/tasks/active/$ARGUMENTS.md"

Read @specs/tasks/active/$ARGUMENTS.md.

If the ## Decisions section is missing or empty,
stop immediately and report: "Task has not been activated. Run task-activate first."

Before planning, ensure you have read the project configuration file
and all documents it references.

Do not ask clarifying questions. Proceed directly to planning.

1. Analyze the codebase to understand the current state relevant to this task.
2. Develop a detailed implementation plan (files to create/modify,
   order of operations, risks).
3. For any open research items, resolve them using Context7
   before finalizing the plan.
4. Add the plan under the ## Implementation Plan section
   of @specs/tasks/active/$ARGUMENTS.md.
5. Show me the completed plan and report: "Plan saved.
   Review it and run task-implement $ARGUMENTS when ready."
6. Do not implement anything.
