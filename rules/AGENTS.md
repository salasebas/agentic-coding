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

<!-- context7 -->
Use Context7 MCP to fetch current documentation whenever the user asks about a library, framework, SDK, API, CLI tool, or cloud service — even well-known ones like React, Next.js, Prisma, Express, Tailwind, Django, or Spring Boot. This includes API syntax, configuration, version migration, library-specific debugging, setup instructions, and CLI tool usage. Use even when you think you know the answer — your training data may not reflect recent changes. Prefer this over web search for library docs.

Do not use for: refactoring, writing scripts from scratch, debugging business logic, code review, or general programming concepts.

## Steps

1. Always start with `resolve-library-id` using the library name and what to look up in the library's documentation, unless the user provides an exact library ID in `/org/project` format
2. Pick the best match (ID format: `/org/project`) by: exact name match, description relevance, code snippet count, source reputation (High/Medium preferred), and benchmark score (higher is better). If results don't look right, try alternate names or queries (e.g., "next.js" not "nextjs", or rephrase the question). Use version-specific IDs when the user mentions a version
3. `query-docs` with the selected library ID and what to look up in the library's documentation (not single words), scoped to a single concept. If the question spans multiple distinct concepts (e.g. routing and auth and caching), make a separate `query-docs` call per concept with the same library ID, unless the question is about how the concepts interact — combined queries dilute ranking and return shallow results for each topic
4. Answer using the fetched docs
<!-- context7 -->
