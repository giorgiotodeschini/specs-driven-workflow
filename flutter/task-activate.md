# task-activate

Analyzes the Flutter codebase, resolves task ambiguities, and documents architectural decisions before implementation.

## Usage

```
task-activate <task-filename>
```

The task file must be located in `specs/tasks/backlog/`.

---

## Objective

Transform a task from the backlog into a task ready for scheduling. Upon completion, the file will have the **Notes** and **Decisions** sections filled in, the status updated to `active`, and will be moved to `specs/tasks/active/`.

---

## Phase 1 — Context Reading

Read in order:

1. The indicated task file
2. `specs/flutter/flutter-architecture.md`
3. `specs/flutter/flutter-data-models.md`
4. `specs/flutter/flutter-routing.md`
5. `specs/flutter/flutter-widgets.md`
6. `specs/flutter/flutter-howto.md`
7. `pubspec.yaml`

If some specs files don't yet exist, report this in the task Notes and proceed with the available information.

---

## Phase 2 — Codebase Analysis

Perform analyses based on the task type. For each analysis, document the results in the **Notes**.

### Common Analysis (All Types)

**Declared Dependencies**
- Verify that all required pub.dev dependencies are already in `pubspec.yaml`
- Identify any missing dependencies with the compatible version
- Report version conflicts with existing constraints

**Affected Files**
- Identify Dart files that will be created, modified, or deleted
- Estimate the impact (surgical / medium / transversal)

**Conflicts with Active Tasks**
- Read tasks in `specs/tasks/active/`
- Report any file or state slice overlaps

---

### Analysis by `widget` type

- Search for similar widgets in `lib/` (name, structure, parameters)
- Evaluate whether an existing widget can be extended instead of creating a new one
- Verify whether the widget interacts with the Riverpod provider, and which one
- Identify any existing golden tests for related widgets in `test/`
- Check whether the widget should be exported from a barrel file

### Analysis by `screen` type

- Check existing routing in `router.dart` (or equivalent go_router)
- Identify the proposed path and check for conflicts with existing routes
- Map the required Riverpod providers: do they already exist? Should they be created?
- Check authentication management (guard, redirect)
- Identify reusable scaffolds/base layouts for this screen

### Analysis by `state` type

- Map existing Riverpod providers related to the state slice
- Check if a partial provider already exists to extend
- Identify all consumers (widgets, other notifiers) that depend on this state
- Analyze side effects: repositories, services, local storage involved
- Check if data model changes are needed (this will potentially trigger a `gen` task)

### Analysis by `platform` type

- Read `android/app/src/main/AndroidManifest.xml` and `ios/Runner/Info.plist`
- Check the minimum Android (`minSdkVersion`) and iOS (`IPHONEOS_DEPLOYMENT_TARGET`) versions
- Search for already used pub.dev plugins that might cover the requirement
- If native code is needed, check for existing MethodChannels
- Identify existing permissions vs. permissions to be added

### Analysis by `theme` type

- Read the project's `ThemeData` file (typically `lib/core/theme/`)
- Identify related existing tokens (colors, typography, spacing)
- Check the theme's custom extension (if present) (`ThemeExtension`)
- Check whether dark mode is implemented and how

### Analysis by `test` type

- Identify existing test files related to the code to be covered
- Check the `test/` directory structure and conventions used
- Identify existing reusable helpers and mocks
- For golden tests: Check the `golden_toolkit` version or equivalent

### Analysis by `gen` type

- Read `build.yaml` if present
- Identify all annotated classes involved (`@freezed`, `@JsonSerializable`, `@riverpod`)
- Check which files `.g.dart` / `.freezed.dart` already exist and which ones need to be regenerated.
- Identify active or backlogged tasks that depend on the generated files.

### Analysis by `bugfix` type

- Search the codebase for the exact location of the incorrect behavior.
- Identify whether the bug is isolated or a symptom of a larger problem.
- Verify whether tests exist that should have caught the bug.

### Analysis by `refactor` type

- Map all files that import or use the classes/widgets to be refactored.
- Verify whether there are any exposed public APIs (widget params, API providers) that will change.
- Identify tests to update after the refactor.

### Analysis by `spike` type

- Search for similar experiments or branches already in the project.
- Identify candidate pub.dev dependencies and their known limitations.
- Document the evaluation criteria to be used in the spike.

---

## Phase 3 — Ambiguity Resolution

For each task point that is ambiguous after When analyzing the codebase, ask the user a direct question. Do not proceed further until critical ambiguities are resolved.

Resolved.

Examples of typical Flutter ambiguities:
- "Should the widget be `const`-constructable or maintain internal state?"
- "Should the provider be `autoDispose` or persist across the session?"
- "Should the route be part of the existing ShellRoute or standalone?"
- "Should the `widgets.dart` barrel file be updated, or is the widget internal to the feature?"

---

## Step 4 — Documenting Decisions

Fill in the **Decisions** section of the task with the architectural decisions made during the analysis. Use this format for each decision:

```markdown
### D{N}: {Short title of the decision}
**Context**: {why this decision had to be made}
**Decision**: {what was decided}
**Rationale**: {why this option versus alternatives}
**Consequences**: {impact on the codebase or other tasks}
```

Typical decisions to document for Flutter:
- Choice of Riverpod provider type (`Provider`, `NotifierProvider`, `AsyncNotifierProvider`, `StreamNotifierProvider`)
- `autoDispose` vs. persistent
- Widget position in the package/feature tree
- Testing strategy (which test types, mock vs. fake)
- Pub.dev dependency added or dropped (with rationale)
- Navigation approach (push, go, pushReplacement)

---

## Phase 5 — Update the Task

1. Fill in the **Notes** section with the results of the codebase analysis
2. Fill in the **Decisions** section with the documented decisions
3. Update **Dependencies** if the analysis revealed unreported dependencies
4. Change **Status** from `backlog` to `active`
5. Move the file from `specs/tasks/backlog/` to `specs/tasks/active/`

---

## Step 6 — Confirmation

Show the user a summary of:
- Files that will be involved in the implementation
- Pub.dev dependencies to be added (if any)
- Key decisions made
- Any risks or warnings identified

Suggest the next command: `task-plan {task-file-name}`.