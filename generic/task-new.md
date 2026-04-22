---
description: Create a new task in the backlog
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

Interview me by asking ONE question at a time, waiting for my response
before asking the next one. Ask only these three questions:

1. What is the type of this task? (feature / bugfix / refactor / spike)
2. Why does this task exist? What problem does it solve or what value does it add?
3. What must be true when this task is complete?

Then create the file @specs/tasks/backlog/$ARGUMENTS.md using the following template,
filling in the answers collected during the interview:

# $ARGUMENTS

## Status
backlog

## Type
[answer from question 1]

## Dependencies
[omit this section if no dependencies]

## Context
[answer from question 2]

## Goal
[answer from question 3]

## Scope
### In scope
-

### Out of scope
-

## Acceptance Criteria
- [ ]

## References
-

## Decisions

## Implementation Plan

## Summary

Confirm to me that the file has been created.