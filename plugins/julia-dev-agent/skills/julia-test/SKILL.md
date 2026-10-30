---
name: julia-test
description: Create or update Julia test cases for current changes using the repository's existing test framework, fixtures, naming, organization, and execution standards, then run and report the tests.
argument-hint: "[change, module, file, or behavior]"
disable-model-invocation: true
user-invocable: true
model: inherit
effort: high
---

# Julia test developer

Create or update tests for: `$ARGUMENTS`

## Workflow

1. Read repository guidance, `Project.toml`, test configuration, CI workflows, and the changed implementation.
2. Read representative existing tests in the same area to identify framework, helpers, fixtures, naming, setup/teardown, and assertion style.
3. Derive test cases from the requested behavior and actual diff.
4. Add the smallest maintainable test set that covers relevant categories:
   - expected behavior
   - boundary and edge cases
   - invalid input and error behavior
   - empty, missing, `nothing`, or `missing` values when applicable
   - regressions for the reported defect
   - compatibility/public API behavior
   - numerical tolerance and determinism where applicable
   - concurrency or integration behavior only when relevant and supported by repository fixtures
5. Avoid tests coupled to private implementation details unless repository practice requires it.
6. Do not call real production services or use production data.
7. Run the narrowest relevant test target first, then the standard Julia test command used by the repository.
8. Run formatter/lint checks for test files when configured.
9. If a failure reveals an implementation defect, do not hide it by weakening the assertion. Report the defect and provide a `/julia-dev` correction command.

## Completion response

Return:

- `TEST_SCOPE`
- `TEST_FILES_CHANGED`
- `CASES_ADDED`
- `COMMANDS_RUN`
- `RESULTS`
- `UNCOVERED_RISKS`
- `NEXT_COMMAND`: `/julia-rev` when all required tests pass, otherwise `/julia-dev <specific fix>`

Never claim tests passed unless they were actually executed successfully.
