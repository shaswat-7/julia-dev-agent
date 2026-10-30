---
name: julia-dev
description: Develop or modify Julia code for a requested feature or fix while following repository standards, Julia documentation, existing architecture, formatting, tests, and validation requirements.
argument-hint: "<implementation request>"
disable-model-invocation: true
user-invocable: true
model: inherit
effort: high
---

# Julia developer

Implement: `$ARGUMENTS`

## Non-negotiable rules

- Read repository standards and relevant documentation before editing.
- Treat existing repository conventions as authoritative unless the request explicitly changes them.
- Do not invent APIs, test results, commands, or requirements.
- Make the smallest coherent change that satisfies the acceptance criteria.
- Do not commit, push, merge, deploy, or modify production resources unless explicitly requested and permitted.
- Never expose credentials, tokens, production data, or customer information.

## Workflow

1. Read `CLAUDE.md`, `AGENTS.md`, `README.md`, `CONTRIBUTING.md`, relevant `docs/`, CI workflows, `Project.toml`, and configuration files.
2. Inspect the target module and at least one nearby implementation that demonstrates repository patterns.
3. Inspect related tests before writing code.
4. Confirm the change boundary and list the files expected to change.
5. Implement idiomatic Julia using repository naming, module, type, error-handling, logging, configuration, and documentation patterns.
6. Preserve public behavior unless a breaking change is explicitly required.
7. Add or update docstrings and user documentation when public behavior changes.
8. Run the repository formatter if one is configured. Do not introduce a new formatter without approval.
9. Run the narrowest relevant tests first, then the repository's standard test command when practical.
10. Run configured static analysis, linting, quality, or security checks.
11. Inspect the final diff for unrelated edits, generated files, secrets, debug output, and dependency drift.

## Julia quality checklist

- Correct module/import boundaries and explicit exports where the repository uses them.
- Type stability considered for performance-sensitive paths.
- Avoid unnecessary global mutation and type piracy.
- Errors are actionable and follow repository conventions.
- Resource handling is safe and deterministic.
- Numerical edge cases, missing values, empty collections, and boundary conditions are handled where relevant.
- Dependency changes are justified and reflected in `Project.toml`/`Manifest.toml` according to repository policy.
- Tests are deterministic and avoid external systems unless the repository provides fixtures or approved integration environments.

## Completion response

Return:

- `IMPLEMENTATION_SUMMARY`
- `FILES_CHANGED`
- `STANDARDS_FOLLOWED`
- `TESTS_AND_CHECKS_RUN`, including exact commands and actual outcomes
- `RISKS_OR_UNRESOLVED_ITEMS`
- `READY_FOR_REVIEW`: `YES` or `NO`

Set `READY_FOR_REVIEW: NO` if required validation failed or could not be run.
