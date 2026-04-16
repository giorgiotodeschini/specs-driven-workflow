# Specs-Driven Task Workflow

A task management workflow for AI-assisted development teams. Four custom commands that enforce a structured lifecycle — from task definition to implementation — keeping every decision documented and every phase explicit.

Designed to work with any agent that supports custom slash commands: **Claude Code**, **Cursor**, **Continue**, and others.

---

## The problem this solves

AI coding assistants work best when they operate within a well-defined context. Without structure, the typical flow is:

1. You have an idea
2. You describe it to the agent
3. The agent starts implementing
4. Ambiguities surface mid-implementation
5. You course-correct, the agent adjusts
6. The result is code that's half-planned, half-improvised

This workflow moves all ambiguity resolution to *before* any code is written.

---

## How it works

Every task lives as a Markdown file and moves through three directories:

```
specs/
└── tasks/
    ├── backlog/    ← tasks being defined
    ├── active/     ← approved tasks with implementation plan
    └── done/       ← completed tasks
```

Four commands manage the full lifecycle:

| Command | What it does | Input | Output |
|---|---|---|---|
| `task-new` | Creates a task via a short interview | — | Task file in `backlog/` |
| `task-activate` | Resolves ambiguities, documents decisions | Task in `backlog/` | Task moved to `active/` |
| `task-plan` | Generates implementation plan | Task in `active/` | Plan added to task file |
| `task-implement` | Executes the approved plan | Task in `active/` | Task moved to `done/` |

Each command verifies the previous step completed before proceeding. If a phase is skipped, the command stops with a clear error message.

---

## Guardrail chain

```
task-new        → creates file in backlog/
                  → checks no duplicates in backlog/ active/ done/

task-activate   → checks file exists in backlog/
                  → adds ## Decisions
                  → moves to active/

task-plan       → checks ## Decisions present (task-activate ran)
                  → adds ## Implementation Plan
                  → shows plan, waits for approval

task-implement  → checks ## Decisions present
                  → checks ## Implementation Plan present
                  → implements, moves to done/
                  → writes ## Summary
```

---

## Task file template

Every task follows this structure. Sections are filled progressively by each command.

```markdown
# <task-name>

## Status
backlog | active | done

## Type
feature | bugfix | refactor | spike

## Context
[Why this task exists]

## Goal
[What must be true when complete]

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
[Filled by task-activate]

## Implementation Plan
[Filled by task-plan]

## Summary
[Filled by task-implement]
```

---

## Spike tasks

Not every task produces code. A spike reduces uncertainty before planning.

Examples: *Can we use this library with our architecture? What breaks if we change this aggregate? What's the performance profile of this approach?*

Spike tasks follow a research-oriented workflow and produce `## Findings` instead of `## Summary`. If the spike identifies follow-up work, it generates suggested tasks and creates them in `backlog/` after your confirmation.

---

## Project configuration

Create a `CLAUDE.md` (or `AGENTS.md` depending on your toolchain) at the project root that lists the specs documents the agent should read before any task. Example:

```markdown
## Important: Read These Documents First

Before working on any task, always read:

1. `specs/architecture-patterns.md` — canonical code patterns
2. `specs/domain-model.md` — aggregates, value objects, invariants
3. `specs/infrastructure-patterns.md` — persistence conventions
```

The commands reference this file with:

```
Before analyzing, ensure you have read the project configuration file
and all documents it references.
```

This keeps the commands agnostic to your specific specs structure — update `CLAUDE.md`, all commands adapt automatically.

---

## Versions

### `generic/`
The base workflow. Works with any project and any tech stack.

### `grasshopper/`
Specialized for Grasshopper plugin development (Rhino/Grasshopper, C#). Adds:

- Task types: `new component`, `modify component`, `bugfix`, `refactor`, `spike`
- `task-new` reads available categories/subcategories from `specs/gh-component-howto.md`
- `task-new` filters the component catalog (`specs/grasshopper-components.md`) by keyword for `modify component` and `bugfix`
- `task-activate` formalizes the Component Spec with GH types, Access mode, optionality, and a generated GUID
- `task-plan` scaffolds the component class structure following project conventions

---

## Installation

1. Copy the commands from `generic/` (or `grasshopper/`) to your project's custom commands directory
2. Create the `specs/tasks/backlog/`, `specs/tasks/active/`, and `specs/tasks/done/` directories
3. Create your `CLAUDE.md` with references to your project specs
4. Run `task-new <task-name>` to create your first task

---

## License

[Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE.md)

You are free to use, adapt, and share this workflow for any purpose, including commercially, as long as you give appropriate credit.

**Attribution:** Giorgio Todeschini ([@giorgiotodeschini](https://github.com/giorgiotodeschini))

---

## Author

Giorgio Todeschini
GitHub: [giorgiotodeschini](https://github.com/giorgiotodeschini)
