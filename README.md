# Julia Engineering Skills

Project-scoped Claude Code skills for Julia development, review, analysis, and testing.

## Commands

- `/julia-analyser <request>`: classify the request, inspect the repository, recommend Haiku/Sonnet/Opus, and produce an execution plan.
- `/julia-dev <request>`: implement a Julia change using repository standards, documentation, existing patterns, validation, and tests.
- `/julia-rev [scope]`: review the current Julia changes. Returns `APPROVED` or `CHANGES_REQUIRED` with exact correction commands.
- `/julia-test [scope]`: create or update Julia tests for the current change and run the repository test commands.

Claude Code discovers project skills from `.claude/skills/<name>/SKILL.md`. Run `/skills` to verify they are listed.

## Install

Copy the `.claude` directory into the root of the target Julia repository and commit it.

```bash
cp -R .claude /path/to/julia-repository/
cd /path/to/julia-repository
claude
```

Then run:

```text
/skills
/julia-analyser add validation to the PNR parser
/julia-dev implement the validation described in issue 123
/julia-test src/parser.jl
/julia-rev
```

## Important routing note

Claude Code supports a static `model` value in skill frontmatter for the current turn. A single skill cannot reliably rewrite its own frontmatter after it has started. Therefore `julia-analyser` classifies the task and recommends `haiku`, `sonnet`, or `opus`. If the current Claude Code environment supports model-specific subagent delegation, it may delegate using that recommendation. Otherwise it prints the `/model` command to run before invoking the next skill.

Suggested policy:

- **Haiku**: lookup, formatting, simple documentation, localized mechanical edits with a clear pattern.
- **Sonnet**: normal feature work, tests, refactoring, debugging, and repository-aware implementation.
- **Opus**: ambiguous architecture, cross-package redesign, concurrency correctness, difficult numerical reasoning, or high-risk migration.

## Review gate

`julia-rev` is intentionally non-editing. It must not approve a change when a blocking issue exists. It returns exact commands and file-level corrections, after which rerun `/julia-dev`, `/julia-test`, and `/julia-rev`.

## Repository standards

The skills discover standards in this order:

1. `CLAUDE.md` and `AGENTS.md`
2. `README.md`, `CONTRIBUTING.md`, and `docs/`
3. `Project.toml`, `Manifest.toml`, and package layout
4. `.JuliaFormatter.toml`, lint/static-analysis configuration, CI workflows, and Makefiles
5. Existing nearby source and test files

Repository-specific rules override generic Julia guidance.


## Julia Engineer Agent

The `.claude/agents/julia-engineer.md` agent orchestrates all four skills as an end-to-end pipeline:

```text
request -> julia-analyser -> julia-dev -> julia-test -> julia-rev
                                      ^                   |
                                      |-- corrections ----|
```

The reviewer is the final gate. A `CHANGES_REQUIRED` result is routed back to development, retested, and reviewed again before approval.
