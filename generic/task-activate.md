---
description: Activate the task
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

Do NOT implement or develop the implementation plan yet.

Before analyzing, ensure you have read the project configuration file
and all documents it references.

1. Analyze the codebase to understand the context of the task.
2. Read the specs documents relevant to this task based on what you found in step 1.
3. Identify any ambiguous, unspecified, or aspects that require a design decision.
4. Interview me by asking ONE question at a time, waiting for my response
   before asking the next one.
5. At the end of the interview, update @specs/tasks/backlog/$ARGUMENTS.md by:
   - Filling in any missing sections of the task
   - Adding a ## Decisions section with each decision taken during
     the interview formatted as:
     **[topic]**: [choice made] — [reason]
   Then move the file to specs/tasks/active/
