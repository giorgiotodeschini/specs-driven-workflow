# task-plan

Generates a detailed implementation plan for an active task and awaits explicit approval before proceeding.

## Usage

```
task-plan <task-filename>
```

The task file must be located in `specs/tasks/active/`.

---

## Objective

Produce a sequence of atomic, ordered steps that an agent can mechanically execute with `task-implement`. The plan must be detailed enough to not require additional architectural decisions during execution.

---

## Preconditions

Verify that the task has:
- Status `active`
- **Decisions** section filled in (not empty)
- Acceptance Criteria defined

If any of these conditions are missing, abort and prompt the user to run `task-activate` first.

---

## Phase 1 — Context Review

Reread:
1. The task file (with Notes and Decisions already compiled)
2. `specs/flutter/flutter-howto.md` — for the project's operational recipes
3. `specs/flutter/flutter-architecture.md` — to confirm the expected structure

---

## Phase 2 — Plan Generation

Generate the **Implementation Plan** section as a numbered list of steps. Each step must follow these rules:

- **Atomic**: A single file created, modified, or deleted; or a single command executed
- **Ordered**: Each step can only depend on previous steps
- **Verifiable**: The state of the codebase after the step is observable
- **Explicit**: The file path is complete, the class/function name is defined

### Format of a Step

```
{N}. [{action}] `{path/file.dart}` — {short description}
- {detail 1}
- {detail 2}
```

Standard actions: `CREATE`, `MODIFY`, `DELETE`, `RUN`, `VERIFY`

---

## Phase 3 — Plan Patterns by Type

Use these patterns as a basis, adapting them to the Decisions of the specific task.

### Plan for `widget`

```
1. [CREATE] `lib/{feature}/widgets/{widget_name}.dart`
- Define the class as StatelessWidget or ConsumerWidget (see Decisions)
- Public parameters with non-nullable types where possible
- Const constructor if applicable

2. [CREATE] `test/widgets/{widget_name}_test.dart`
- Basic rendering test (golden or widget test, see Decisions)
- Edge case parameter test

3. [MODIFY] `lib/{feature}/widgets/widgets.dart` (barrel file)
- Add export of the new widget

4. [VERIFY] Run `flutter test test/widgets/{widget_name}_test.dart`
```

### Plan for `screen`

```
1. [CREATE] `lib/{feature}/screens/{screen_name}.dart`
- ConsumerWidget or ConsumerStatefulWidget structure
- Integration with providers identified in Decisions

2. [MODIFY] `lib/core/router/router.dart`
- Add the route with the path defined in Decisions
- Configure authentication guards if necessary

3. [CREATE] `test/screens/{screen_name}_test.dart`
- Rendering test with mocked providers
- Navigation test

4. [VERIFY] Run `flutter test test/screens/{screen_name}_test.dart`
```

### Plan for `state`

```
1. [CREATE] `lib/{feature}/providers/{provider_name}.dart`
- Provider type from Decisions (NotifierProvider / AsyncNotifierProvider / etc.)
- Notifier class with public methods
- Annotation @riverpod if riverpod_generator is in use

2. [RUN] `dart run build_runner build --delete-conflicting-outputs`
(only if using riverpod_generator)

3. [MODIFY] `{involved repository or service file}`
- Adapt the interface if necessary

4. [CREATE] `test/providers/{provider_name}_test.dart`
- Test initial states
- Test state transitions
- Mock the repository/service

5. [VERIFY] Run `flutter test test/providers/{provider_name}_test.dart`
```

### Plan for `platform`

```
1. [MODIFY] `android/app/src/main/AndroidManifest.xml`
- Add necessary permissions

2. [MODIFY] `ios/Runner/Info.plist`
- Add permission keys Required

3. [CREATE] `lib/core/platform/{service_name}.dart`
- Dart interface for the service
- Implementation with MethodChannel

4. [MODIFY] `android/app/src/main/kotlin/.../MainActivity.kt`
- Register the MethodChannel handler

5. [MODIFY] `ios/Runner/AppDelegate.swift`
- Register the MethodChannel handler

6. [CREATE] `test/platform/{service_name}_test.dart`
- Test with MockMethodChannel

7. [VERIFY] Run `flutter test` + manual verification on device/emulator
```

### Plan for `theme`

```
1. [MODIFY] `lib/core/theme/app_colors.dart` (or equivalent)
- Add/edit token color

2. [MODIFY] `lib/core/theme/app_theme.dart`
- Update ThemeData light and dark

3. [MODIFY] `lib/core/theme/app_theme_extensions.dart` (if it exists)
- Update custom ThemeExtension

4. [VERIFY] Run the app and visually verify light/dark mode
```

### Plan for `gen`

```
1. [MODIFY] `{file with annotated classes}`
- Add/edit @freezed / @JsonSerializable / @riverpod annotations

2. [RUN] `dart run build_runner build --delete-conflicting-outputs`

3. [VERIFY] Check the generated .g.dart / .freezed.dart files for errors

4. [VERIFY] Run `flutter analyze`
```

### Plan for `bugfix`

```
1. [CREATE] `test/{path}_test.dart`
- Write the test that reproduces the bug (must fail)

2. [MODIFY] `{file with the bug}`
- Apply the fix

3. [VERIFY] Run the test created in step 1 (must pass)

4. [VERIFY] Run `flutter test` to verify no regressions
```

### Plan for `test`

```
1. [CREATE] `test/{path}/{name}_test.dart`
- Structure the file with groups by scenario

2. [RUN] `flutter test {test path}`
- Verify that all tests pass

3. [VERIFY] Run `flutter test --coverage` and verify the Coverage threshold
```

### Plan for `refactor`

```
1. [VERIFY] Run `flutter test` — all tests must pass before starting

2. [MODIFY] `{main refactor file}`
- Apply the structural change

3. [MODIFY] `{files importing the refactored code}` (one per step)
- Update dependencies

4. [VERIFY] Run `flutter analyze`

5. [VERIFY] Run `flutter test` — no regressions
```

---

## Phase 4 — Closing Step (all types)

Always add these final steps to the plan:

```
N. [RUN] `flutter analyze`
- Zero errors and zero warnings introduced by the task

N+1. [RUN] `flutter test`
- All existing tests continue to pass

N+2. [VERIFY] Acceptance Criteria
- Manually or automatically verify each criterion defined in the task.
```

---

## Step 5 — Writing the Plan to the Task

Fill in the **Implementation Plan** section of the task file with the generated plan.

---

## Step 6 — Request for Approval

Present the plan to the user in this format:

```
## Plan generated for: {task title}

{list of steps}

---
Risks identified:
- {risk 1, if any}
- {risk 2, if any}

Steps requiring manual attention:
- {step N: reason}

To proceed with implementation, respond: I APPROVE
To modify the plan, indicate the corrections to be made.
```

**Do not proceed further until the user explicitly writes "I APPROVE"** (or an unambiguous equivalent).

If the user requests changes, update the plan and request approval again.

---

## Step 7 — After Approval

1. Update the **Implementation Plan** section in the task file with the approved version.
2. Report: "Plan approved and saved. Run `task-implement {task-filename}` to start implementation."