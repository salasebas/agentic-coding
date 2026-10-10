---
description: Global instructions from ~/.agents/AGENTS.md
alwaysApply: true
---

# AGENTS.md
- English first: all code, comments, identifiers, and internal docs in English.
- Grow the system in layers. Start from the smallest version that works end to end, and add each new capability on top of a product that already works. Never trade a working product for unfinished complexity.
- Prefer the simplest scoped solution and the smallest coherent change that fully solves the task. Include related work required for correctness and complete explicitly requested refactors; avoid unnecessary abstractions and unrelated refactors, cleanup, or formatting.
- Keep components modular and concerns clearly separated.
- Prefer established, well-maintained libraries when they reduce overall complexity or improve reliability. Do not reimplement common functionality without a clear reason.
- Lean on the dependencies already in the project before writing your own implementation or adding packages. Do not assume a library lacks a capability without checking its documentation and types.
- Make architectural decisions for the long term. Do not accept a stopgap that only works for now and is meant to be replaced later.
- When opening a pull or merge request that finishes an issue, put `Closes #<issue>` in the description. The merge closes the issue.

## Documentation
- When writing or revising a README, tutorial, how-to, reference, or explanation, read and follow the `writing-documentation-with-diataxis` skill before editing.

## Engineering Principles
- Diagnose the root cause before implementing a fix, and prefer durable corrections over workarounds or symptom suppression.
- Understand the surrounding implementation before modifying it.

## Package Manager Preferences
- Use `pnpm` instead of `npm` unless the project explicitly requires another package manager.
- Use `uv` for Python package management and virtual environments when compatible with the project.

## Review workflow
- When asked to review changes, use the `review-changes` skill.
- Without the skill, give the reviewer a fresh context containing the original
  requirements, the exact diff scope, and access to relevant code and project
  instructions. Treat each finding as a hypothesis to confirm or refute, not an
  instruction to modify code.
- You must validate every finding before presenting it to the User.
- Reviewers run on the session's model. Review with another model only when
  the User names it.

## Test value
- Before adding tests, identify the repository-owned failure, independent
  expected outcome, and gap in existing coverage.
- Choose the cheapest test layer that reaches the real failure conditions.
  Extend existing coverage when it already owns the scenario.
- Review assertions and fixtures against requirements, not implementation.
  Passing tests or another agent's agreement do not establish correctness.
- Consolidate coverage only after confirming equivalent triggering conditions,
  or retire the obsolete obligation. Preserve meaningful regressions.

## Subagents

- This is a standing request to delegate: use subagents on your own initiative for independent, substantial research, exploration, analysis, review and implementation when doing so keeps the main context focused or improves speed or confidence.
- Give each subagent a focused, bounded task with a clear objective and expected output. Keep tiny or tightly coupled work on the main agent when coordination would cost more than it saves.
- Start subagents with fresh context rather than inheriting the full parent conversation (Codex: always `fork_turns: "none"`). Provide a self-contained brief with the objective, relevant requirements, scope, constraints, and expected output.
- Fan out one subagent per independent task, up to 10 per wave, each writer owning disjoint files.
- Keep pushes, deletions, and other destructive operations on the main agent.
- Scouts and reviewers are read-only and must not edit files.

Route each subagent by role, as model and reasoning effort:

| Role | Claude Code | Codex |
| --- | --- | --- |
| Simple scout: grep, locating files, extracting data, parallel scouting | `haiku`, `medium` | `gpt-6-luna`, `high` |
| Tracing scout: follows a flow across modules or finds the likely cause of a bug | `sonnet`, `medium` | `gpt-6.1-sol`, `low`; `medium` if the question is ambiguous |
| Implementation | `sonnet`, `medium` | `gpt-6.1-sol`, `medium` |
| Reviewer or validator | the session's model, `high` | the session's model, `high` |

- If a simple scout returns inconclusive or shallow results, escalate to the tracing scout instead of retrying it.
- Escalate implementation to `high` when a concrete reasoning blocker remains after inspecting relevant code and checking assumptions.
- Reserve `xhigh` for unresolved, consequential problems where additional reasoning is likely to help. Task size or importance alone is insufficient.
