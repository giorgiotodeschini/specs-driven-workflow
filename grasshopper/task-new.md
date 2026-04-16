---
description: Create a new Grasshopper task in the backlog
agent: build
---

If $ARGUMENTS is empty,
stop immediately and report: "Task name is required. Usage: task-new <task-name>"

If the file @specs/tasks/backlog/$ARGUMENTS.md already exists,
stop immediately and report: "Task already exists in backlog: specs/tasks/backlog/$ARGUMENTS.md"

If the file @specs/tasks/active/$ARGUMENTS.md already exists,
stop immediately and report: "Task already exists in active: specs/tasks/active/$ARGUMENTS.md"

If the file @specs/tasks/done/$ARGUMENTS.md already exists,
stop immediately and report: "Task already exists in done: specs/tasks/done/$ARGUMENTS.md"

Do NOT analyze the codebase or implement anything.

Ask me ONE question at a time, waiting for my response before asking the next one.

---

Start by asking:
"What is the type of this task? (new component / modify component / bugfix / refactor / spike)"

Then follow the interview branch that matches the answer:

---

## BRANCH: new component

1. Ask: "What is the name of the component?"

2. Read @specs/gh-component-howto.md to get the list of available categories
   and subcategories, then ask:
   "In which category and subcategory should it be inserted?"
   Show the available options from the file.

3. Ask: "Describe the inputs the component should have
   (names and meaning only, no GH types needed)."

4. Ask: "Describe the outputs the component should produce
   (names and meaning only, no GH types needed)."

5. Ask: "Do you have any additional notes or constraints for this task?
   (press enter to skip)"

Then create @specs/tasks/backlog/$ARGUMENTS.md with this template:

# $ARGUMENTS

## Status
backlog

## Type
new component

## Component Spec
**Name:** [answer 1]
**Category:** [answer 2]

### Inputs
[answer 3 — one item per line as: - Name: description]

### Outputs
[answer 4 — one item per line as: - Name: description]

## Goal
Implement the [answer 1] Grasshopper component as described in Component Spec.

## Scope
### In scope
-

### Out of scope
-

## Acceptance Criteria
- [ ]

## References
-

## Notes
[include this section with answer 5 only if the user provided a non-empty answer]

## Decisions

## Implementation Plan

## Summary

---

## BRANCH: modify component

1. Ask: "Enter a keyword to search for the component to modify."

2. Read @specs/grasshopper-components.md, filter components whose name contains
   the keyword (case-insensitive), and show the matching list.
   Ask: "Which component do you want to modify?"

3. Ask: "What should change in this component?"

4. Ask: "Do you have any additional notes or constraints for this task?
   (press enter to skip)"

Then create @specs/tasks/backlog/$ARGUMENTS.md with this template:

# $ARGUMENTS

## Status
backlog

## Type
modify component

## Component Spec
**Component:** [answer 2]

## Context
[answer 3]

## Goal
Modify [answer 2] as described in Context.

## Scope
### In scope
-

### Out of scope
-

## Acceptance Criteria
- [ ]

## References
-

## Notes
[include this section with answer 4 only if the user provided a non-empty answer]

## Decisions

## Implementation Plan

## Summary

---

## BRANCH: bugfix

1. Ask: "Enter a keyword to search for the affected component."

2. Read @specs/grasshopper-components.md, filter components whose name contains
   the keyword (case-insensitive), and show the matching list.
   Ask: "Which component is affected?"

3. Ask: "Describe the incorrect behavior."

4. Ask: "Do you have any additional notes or constraints for this task?
   (press enter to skip)"

Then create @specs/tasks/backlog/$ARGUMENTS.md with this template:

# $ARGUMENTS

## Status
backlog

## Type
bugfix

## Component Spec
**Component:** [answer 2]

## Context
[answer 3]

## Goal
Fix the incorrect behavior described in Context in [answer 2].

## Scope
### In scope
-

### Out of scope
-

## Acceptance Criteria
- [ ]

## References
-

## Notes
[include this section with answer 4 only if the user provided a non-empty answer]

## Decisions

## Implementation Plan

## Summary

---

## BRANCH: refactor / spike

1. Ask: "Why does this task exist? What problem does it solve or what value does it add?"

2. Ask: "What must be true when this task is complete?"

3. Ask: "Do you have any additional notes or constraints for this task?
   (press enter to skip)"

Then create @specs/tasks/backlog/$ARGUMENTS.md with this template:

# $ARGUMENTS

## Status
backlog

## Type
[refactor or spike]

## Context
[answer 1]

## Goal
[answer 2]

## Scope
### In scope
-

### Out of scope
-

## Acceptance Criteria
- [ ]

## References
-

## Notes
[include this section with answer 3 only if the user provided a non-empty answer]

## Decisions

## Implementation Plan

## Summary

---

Confirm to me that the file has been created.
