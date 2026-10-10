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

# Package Manager Preferences
- Use `pnpm` instead of `npm` unless the project explicitly requires another package manager.
- Use `uv` for Python package management and virtual environments when compatible with the project.

## Review workflow
- Give the reviewer a fresh context containing the original requirements,
  the exact diff scope, and access to relevant code and project instructions.
- Request actionable correctness, regression, readability & simplicity, architecture, security, and performance findings. Require a concrete failure scenario, location, impact, and evidence.
  Separate optional suggestions from required fixes.
  Zero findings is an acceptable outcome.
- Deduplicate findings before validation. Treat each finding as a hypothesis
  to confirm or refute, not an instruction to modify code.
- Validate small findings locally. Delegate substantial independent groups
  to fresh read-only subagents with bounded objectives.
- You must validate every findings before presenting them to the User.

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

v1: Default: keep work on the main agent. Do not spawn unless it clearly
improves focus, speed, or confidence.

- Delegate substantial, independent work when it improves focus, speed, or confidence; handle small or tightly coupled tasks locally. Give each subagent a bounded objective and expected output.
- Assign non-overlapping write ownership. Keep delegation and integration with the coordinator, who remains available to the user.
- Start subagents with fresh context rather than inheriting the full parent conversation. Provide a self-contained brief with the objective, relevant requirements, scope, constraints, and expected output.

v2: - Use subagents for independent, substantial research, exploration, analysis and implementation when doing so keeps the main context focused or improves speed or confidence.
- Give each subagent a focused, bounded task with a clear objective and expected output. Keep tiny or tightly coupled work on the main agent when coordination would cost more than it save$
- Spawn subagents without inherit parent's context.
- Fan out one subagent per independent task, up to 10 per wave, each writer owning disjoint files.
- Keep pushes, deletions, and other destructive operations on the main agent.

### Subagents (Codex)
- Use `gpt-6-luna` with `reasoning_effort` `"high"` for simple read-only scouting: grep, locating files, extracting data, and parallel scouting.
- Use `gpt-6.1-sol` with `reasoning_effort` `"low"` for read-only scouts that must trace flows across modules or find the likely cause of a bug. Use `"medium"` if the question is ambiguous.
- Use `gpt-6.1-sol` with reasoning effort `"medium"` for implementation.
  Escalate to `"high"` when a concrete reasoning blocker remains after
  inspecting relevant code and checking assumptions.
- Reserve `"xhigh"` for unresolved, consequential problems where additional
  reasoning is likely to help. Task size or importance alone is insufficient.
- If a Luna scout returns inconclusive or shallow results, escalate to `gpt-6.1-sol` `"low"` instead of retrying Luna.
- Scouts and reviews are read-only and must not edit files.
- Always use `fork_turns: "none"`.

<!-- context7 -->
Use Context7 MCP to fetch current documentation whenever the user asks about a library, framework, SDK, API, CLI tool, or cloud service — even well-known ones like React, Next.js, Prisma, Express, Tailwind, Django, or Spring Boot. This includes API syntax, configuration, version migration, library-specific debugging, setup instructions, and CLI tool usage. Use even when you think you know the answer — your training data may not reflect recent changes. Prefer this over web search for library docs.

Do not use for: refactoring, writing scripts from scratch, debugging business logic, code review, or general programming concepts.

## Steps

1. Always start with `resolve-library-id` using the library name and what to look up in the library's documentation, unless the user provides an exact library ID in `/org/project` format
2. Pick the best match (ID format: `/org/project`) by: exact name match, description relevance, code snippet count, source reputation (High/Medium preferred), and benchmark score (higher is better). If results don't look right, try alternate names or queries (e.g., "next.js" not "nextjs", or rephrase the question). Use version-specific IDs when the user mentions a version
3. `query-docs` with the selected library ID and what to look up in the library's documentation (not single words), scoped to a single concept. If the question spans multiple distinct concepts (e.g. routing and auth and caching), make a separate `query-docs` call per concept with the same library ID, unless the question is about how the concepts interact — combined queries dilute ranking and return shallow results for each topic
4. Answer using the fetched docs
<!-- context7 -->
