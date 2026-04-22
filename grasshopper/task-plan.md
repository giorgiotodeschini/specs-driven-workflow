---
description: Generate the implementation plan of a Grasshopper task
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

Then follow the planning branch that matches ## Type:

---

## BRANCH: new component

1. Read @specs/gh-component-howto.md, @specs/data-models.md,
   and @specs/grasshopper-components.md.

2. Analyze the codebase to find existing components similar to the one
   described in ## Component Spec. Use them as reference for structure
   and patterns.

3. Develop a detailed implementation plan following the conventions in
   gh-component-howto.md, including:
   - File to create and its location in the project structure
   - Class skeleton (component class, input/output registration,
     SolveInstance structure)
   - Data models and types to use from data-models.md
   - Error handling and null input strategy as per ## Decisions
   - Rhino preview implementation if required
   - Any helper or utility classes needed
   - Order of operations and risks

4. For any open research items, resolve them using Context7
   before finalizing the plan.

5. Add the plan under the ## Implementation Plan section
   of @specs/tasks/active/$ARGUMENTS.md.

6. Show me the completed plan and report: "Plan saved.
   Review it and run task-implement $ARGUMENTS when ready."

7. Do not implement anything.

---

## BRANCH: modify component

1. Read @specs/gh-component-howto.md, @specs/data-models.md,
   and @specs/grasshopper-components.md.

2. Locate and analyze the source code of the component named
   in ## Component Spec.

3. Develop a detailed implementation plan including:
   - Exact lines or methods to modify
   - Impact on existing inputs/outputs and backward compatibility
   - Any related components or shared code affected by the change
   - Order of operations and risks

4. For any open research items, resolve them using Context7
   before finalizing the plan.

5. Add the plan under the ## Implementation Plan section
   of @specs/tasks/active/$ARGUMENTS.md.

6. Show me the completed plan and report: "Plan saved.
   Review it and run task-implement $ARGUMENTS when ready."

7. Do not implement anything.

---

## BRANCH: bugfix

1. Read @specs/gh-component-howto.md and @specs/data-models.md.

2. Locate and analyze the source code of the component named
   in ## Component Spec.

3. Using the ## Root Cause section as the starting point, develop
   a detailed fix plan including:
   - Exact location of the bug in the source code
   - The fix to apply and why it resolves the root cause
   - Any related components or shared code that may need updating
   - Regression risks and how to mitigate them

4. For any open research items, resolve them using Context7
   before finalizing the plan.

5. Add the plan under the ## Implementation Plan section
   of @specs/tasks/active/$ARGUMENTS.md.

6. Show me the completed plan and report: "Plan saved.
   Review it and run task-implement $ARGUMENTS when ready."

7. Do not implement anything.

---

## BRANCH: refactor / spike

1. Read @specs/gh-component-howto.md and @specs/data-models.md.

2. Analyze the codebase to understand the current state relevant
   to this task.

3. Develop a detailed implementation plan (files to create/modify,
   order of operations, risks).

4. For any open research items, resolve them using Context7
   before finalizing the plan.

5. Add the plan under the ## Implementation Plan section
   of @specs/tasks/active/$ARGUMENTS.md.

6. Show me the completed plan and report: "Plan saved.
   Review it and run task-implement $ARGUMENTS when ready."

7. Do not implement anything.