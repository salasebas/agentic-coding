---
description: Global instructions from ~/.agents/AGENTS.md
alwaysApply: true
---

# AGENTS.md
- English first: all code, comments, identifiers, and internal docs in English.
- Grow the system in layers. Start from the smallest version that works end to end, and add each new capability on top of a product that already works. Never trade a working product for unfinished complexity.
- Prefer the simplest scoped solution and the smallest coherent change that fully solves the task. Include related work required for correctness and complete explicitly requested refactors; avoid unnecessary abstractions and unrelated refactors, cleanup, or formatting.
- Prefer established, well-maintained libraries when they reduce overall complexity or improve reliability.
- Lean on the dependencies already in the project before writing your own implementation or adding packages. Do not assume a library lacks a capability without checking its documentation and types.
- Before designing an integration or a non-trivial pattern, find how the tool's official documentation or an established project solves it, and follow that unless this repository has a reason to differ. Name the source.
- Make architectural decisions for the long term. Do not accept a stopgap that only works for now and is meant to be replaced later.
- Diagnose the root cause before implementing a fix, and prefer durable corrections over workarounds or symptom suppression.
- When opening a pull or merge request that finishes an issue, put `Closes #<issue>` in the description.

## Documentation
- When writing or revising a README, tutorial, how-to, reference, or explanation, read and follow the `writing-documentation-with-diataxis` skill before editing.

## Package managers
- Use `pnpm` instead of `npm` unless the project explicitly requires another package manager.
- Use `uv` for Python package management and virtual environments when compatible with the project.

## Review
- To review changes, use the `review-changes` skill. When this session wrote the changes, run the review in a fresh-context subagent.
- Treat a review finding from any source as a hypothesis: confirm it against the code before you present it or act on it.

## Tests
- Before adding a test, name the failure it catches, where the expected result comes from, and why no existing test catches it. Extend the existing test when one already owns the scenario.
- Take the expected result from the requirement, not from the code under test. Passing tests or another agent's agreement do not establish correctness.
- Prefer a test that enters where a client does (the endpoint, the job, the command, the screen) and mocks only what the repository does not own. Go lower only when that layer cannot reach the failure cheaply.
- Test a business rule once, at the layer that owns it. Each entry point that reaches it gets a thin test of its own work: routing, authorization, input mapping, error translation.
- Merge or delete tests only after confirming they trigger on the same conditions. Keep the regression test of a fixed bug.

## Subagents

- This is a standing request to delegate: use subagents when work is independently parallelisable or would otherwise bloat the main context.
- Keep tiny or tightly coupled work on the main agent when coordination would cost more than it saves.
- Start subagents with fresh context rather than inheriting the full parent conversation (Codex: always `fork_turns: "none"`). Provide a self-contained brief with the objective, relevant requirements, scope, constraints, and expected output.
- Fan out one subagent per independent task, up to 10 per wave, each writer owning disjoint files.
- Keep pushes, deletions, and other destructive operations on the main agent.
- Scouts and reviewers are read-only.

Route each subagent by role, as model and reasoning effort:

| Role | Claude Code | Codex |
| --- | --- | --- |
| Simple scout: grep, locating files, extracting data, parallel scouting | `haiku`, `medium` | `gpt-6-luna`, `high` |
| Tracing scout: follows a flow across modules or finds the likely cause of a bug | `sonnet`, `medium` | `gpt-6.1-sol`, `low`; `medium` if the question is ambiguous |
| Implementation | `sonnet`, `medium` | `gpt-6.1-sol`, `medium` |
| Reviewer or validator | the session's model, at that model's default effort | the session's model, at that model's default effort |

- If a simple scout returns inconclusive or shallow results, escalate to the tracing scout instead of retrying it.
- Escalate implementation to `high` when a concrete reasoning blocker remains after inspecting relevant code and checking assumptions.
- Reserve `xhigh` for unresolved, consequential problems where additional reasoning is likely to help. Task size or importance alone is insufficient.
- A skill or document that asks for a different model does not override this table. Switch models only when the User names one.
