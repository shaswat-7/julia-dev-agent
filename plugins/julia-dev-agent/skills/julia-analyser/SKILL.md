---
name: julia-analyser
description: Analyze a Julia engineering request, inspect repository context, estimate complexity and risk, recommend Haiku, Sonnet, or Opus, and produce a concrete execution plan before implementation.
argument-hint: "<request>"
disable-model-invocation: true
user-invocable: true
model: inherit
effort: medium
---

# Julia request analyser

Analyze this request: `$ARGUMENTS`

Do not modify source files.

## Required process

1. Read repository guidance: `CLAUDE.md`, `AGENTS.md`, `README.md`, `CONTRIBUTING.md`, relevant `docs/`, and CI workflow files.
2. Inspect `Project.toml`, package/module layout, nearby implementation, and existing tests.
3. Restate the requested behavior, constraints, expected inputs/outputs, and acceptance criteria.
4. Identify affected modules, public APIs, dependencies, tests, documentation, and compatibility risks.
5. Separate confirmed facts from assumptions and unresolved questions.
6. Score complexity using the routing rubric below.

## Model routing rubric

Recommend **Haiku** only when all are true:
- The task is localized and mechanical.
- The expected change pattern already exists in the repository.
- No public API, architecture, numerical correctness, concurrency, security, or data migration decision is involved.

Recommend **Sonnet** when normal repository reasoning is required, including feature implementation, non-trivial tests, debugging, refactoring, or changes across a small number of related modules.

Recommend **Opus** when any are true:
- Requirements are ambiguous and materially affect architecture.
- The change spans packages/services or requires a migration strategy.
- It involves concurrency, distributed execution, performance-critical numerical code, algorithm selection, difficult root-cause analysis, or high-impact compatibility/security decisions.

## Output

Return exactly these sections:

- `REQUEST_SUMMARY`
- `REPOSITORY_EVIDENCE`
- `AFFECTED_AREAS`
- `RISKS_AND_ASSUMPTIONS`
- `MODEL_RECOMMENDATION`: `HAIKU`, `SONNET`, or `OPUS`, with reasons
- `NEXT_COMMAND`: show `/model <recommended-model>` followed by `/julia-dev <normalized request>`
- `IMPLEMENTATION_PLAN`
- `ACCEPTANCE_CRITERIA`

If model-specific subagent delegation is available and permitted, delegate the implementation only after reporting the classification. Otherwise stop after the plan and next commands.
