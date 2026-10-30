---
name: julia-rev
description: Review Julia changes against the request, repository standards, architecture, style, tests, documentation, and configured quality gates; reject non-compliant changes with exact correction commands.
argument-hint: "[branch, diff, files, or requirement]"
disable-model-invocation: true
user-invocable: true
model: inherit
effort: high
---

# Julia reviewer

Review scope: `$ARGUMENTS`

This is a review gate. Do not edit files. Do not approve while any blocking finding remains.

## Review process

1. Read the original requirement from the supplied scope and available conversation/repository context.
2. Read repository standards, relevant docs, CI workflows, `Project.toml`, formatting/static-analysis configuration, and nearby implementation patterns.
3. Inspect the complete diff and all directly affected call sites.
4. Check behavior, correctness, architecture, public API compatibility, error handling, logging, configuration, dependencies, documentation, security, and Julia-specific quality.
5. Inspect tests for meaningful coverage rather than line coverage alone.
6. Run read-only validation commands allowed by the repository, including tests and configured formatting/lint checks. Record actual results.
7. Classify every finding:
   - `BLOCKER`: correctness, security, data loss, broken API, failing required check, or requirement not met.
   - `MAJOR`: repository-standard violation, missing important test, maintainability/performance risk requiring correction.
   - `MINOR`: useful improvement that does not block acceptance.
8. Reconcile the implementation against each acceptance criterion.

## Gate decision

Return `CHANGES_REQUIRED` if any `BLOCKER` or `MAJOR` exists, a required command fails, or necessary validation was not performed without a justified reason.

Return `APPROVED` only when the implementation satisfies the request and repository standards and required checks pass.

## Required output

- `REVIEW_DECISION`: `APPROVED` or `CHANGES_REQUIRED`
- `REQUIREMENT_COVERAGE`
- `VALIDATION_RUNS`: exact commands and outcomes
- `FINDINGS`: severity, file and line/range, evidence, required correction
- `CORRECTION_COMMANDS`: exact next commands, for example:
  - `/julia-dev <specific correction request>`
  - `/julia-test <specific missing coverage>`
  - repository formatter/test/lint commands discovered from the repo
- `REVIEWER_STOP`: `YES` when changes are required; state that approval, commit, merge, and deployment must not proceed

Never claim a command passed unless it was actually run successfully.
