# Repository AI Instructions

Use the project skills under `.claude/skills/` for Julia work.

Recommended sequence:

1. `/julia-analyser <request>`
2. Select the recommended model when available.
3. `/julia-dev <request>`
4. `/julia-test <scope>`
5. `/julia-rev <scope>`

A `CHANGES_REQUIRED` result from `/julia-rev` is a hard gate. Apply its correction commands and rerun tests and review. Never fabricate command results, bypass repository quality gates, or expose secrets.
