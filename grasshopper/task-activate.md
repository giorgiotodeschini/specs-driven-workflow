---
description: Activate a Grasshopper task
agent: build
---

If $ARGUMENTS is empty,
read the @specs/tasks/backlog/ directory, list all .md files found,
and report: "No task specified. Available tasks in backlog:
[list of filenames without extension]
Usage: task-activate <task-name>"
Then stop.

If the file @specs/tasks/backlog/$ARGUMENTS.md does not exist,
stop immediately and report: "Task not found: specs/tasks/backlog/$ARGUMENTS.md"

Read @specs/tasks/backlog/$ARGUMENTS.md.

If the ## Dependencies section is present and not empty,
for each dependency listed check if the corresponding file exists
in @specs/tasks/done/.
If any dependency file is NOT in specs/tasks/done/,
stop immediately and report:
"Task cannot be activated. The following dependencies are not done yet:
[list of blocking task names]
Complete them first."

Do NOT implement or develop the implementation plan yet.

Before analyzing, ensure you have read the project configuration file
and all documents it references.

Then follow the activation branch that matches ## Type:

---

## BRANCH: new component

1. Read @specs/gh-component-howto.md, @specs/data-models.md,
   and @specs/grasshopper-components.md.

2. Analyze the codebase to find existing components similar to the one
   described in ## Component Spec. Use them as reference for conventions,
   patterns, and GH types in use.

3. For each input and output listed in ## Component Spec:
   - Attempt to deduce the GH type, Access mode, and optionality
     from the name and description.
   - Ask me only for inputs/outputs where deduction is ambiguous
     or uncertain, ONE question at a time.

4. Identify any ambiguous, unspecified, or aspects that require
   a design decision (behavior with null inputs, list vs item handling,
   Rhino preview, error messaging, edge cases).
   Interview me by asking ONE question at a time, waiting for my response
   before asking the next one.

5. At the end of the interview, update @specs/tasks/backlog/$ARGUMENTS.md by:
   - Filling in any missing sections
   - Replacing ## Component Spec with the fully formalized version:

## Component Spec
**Name:**
**Category:**
**Subcategory:**
**Guid:** [generate a new UUID]

### Inputs
| # | Name | Type | Access | Optional |
|---|------|------|--------|----------|

### Outputs
| # | Name | Type |
|---|------|------|

   - Adding a ## Decisions section with each decision taken during
     the interview formatted as:
     **[topic]**: [choice made] — [reason]

   Then move the file to specs/tasks/active/

---

## BRANCH: modify component

1. Read @specs/grasshopper-components.md and locate the entry for the
   component named in ## Component Spec.

2. Read @specs/gh-component-howto.md and @specs/data-models.md.

3. Locate and analyze the source code of the component in the codebase.

4. Identify any ambiguous, unspecified, or aspects of the requested change
   that require a design decision (impact on existing inputs/outputs,
   backward compatibility, edge cases).
   Interview me by asking ONE question at a time, waiting for my response
   before asking the next one.

5. At the end of the interview, update @specs/tasks/backlog/$ARGUMENTS.md by:
   - Filling in any missing sections
   - Adding a ## Decisions section with each decision taken during
     the interview formatted as:
     **[topic]**: [choice made] — [reason]

   Then move the file to specs/tasks/active/

---

## BRANCH: bugfix

1. Read @specs/grasshopper-components.md and locate the entry for the
   component named in ## Component Spec.

2. Read @specs/gh-component-howto.md and @specs/data-models.md.

3. Locate and analyze the source code of the component in the codebase.

4. Analyze the reported incorrect behavior in ## Context against the
   source code to identify the likely cause.

5. Identify any ambiguous or unspecified aspects that require clarification
   before planning a fix (reproduction steps, expected vs actual behavior,
   related components that may be affected).
   Interview me by asking ONE question at a time, waiting for my response
   before asking the next one.

6. At the end of the interview, update @specs/tasks/backlog/$ARGUMENTS.md by:
   - Filling in any missing sections
   - Adding a ## Root Cause section with the identified cause of the bug
   - Adding a ## Decisions section with each decision taken during
     the interview formatted as:
     **[topic]**: [choice made] — [reason]

   Then move the file to specs/tasks/active/

---

## BRANCH: refactor / spike

1. Read @specs/gh-component-howto.md and @specs/data-models.md.

2. Analyze the codebase to understand the context of the task.

3. Read the specs documents relevant to this task based on what you
   found in step 2.

4. Identify any ambiguous, unspecified, or aspects that require
   a design decision.
   Interview me by asking ONE question at a time, waiting for my response
   before asking the next one.

5. At the end of the interview, update @specs/tasks/backlog/$ARGUMENTS.md by:
   - Filling in any missing sections
   - Adding a ## Decisions section with each decision taken during
     the interview formatted as:
     **[topic]**: [choice made] — [reason]

   Then move the file to specs/tasks/active/