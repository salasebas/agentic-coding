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

## Subagents

- Use subagents for independent, substantial research, exploration, analysis and implementation when doing so keeps the main context focused or improves speed or confidence.
- Give each subagent a focused, bounded task with a clear objective and expected output. Keep tiny or tightly coupled work on the main agent when coordination would cost more than it saves.
- Spawn subagents without inherit parent's context.

## Engineering Principles

- Diagnose the root cause before implementing a fix, and prefer durable corrections over workarounds or symptom suppression.
- Understand the surrounding implementation before modifying it.

# Package Manager Preferences

- Use `pnpm` instead of `npm` unless the project explicitly requires another package manager.
- Use `uv` for Python package management and virtual environments when compatible with the project.

## Multi-Agent Rules (V2) (ONLY APLIES TO CODEX)

- Spawn subagents with `fork_turns: "none"` by default so the configured subagent model applies. Use the smallest positive value when recent conversation context is necessary.

<!-- context7 -->
Use Context7 MCP to fetch current documentation whenever the user asks about a library, framework, SDK, API, CLI tool, or cloud service — even well-known ones like React, Next.js, Prisma, Express, Tailwind, Django, or Spring Boot. This includes API syntax, configuration, version migration, library-specific debugging, setup instructions, and CLI tool usage. Use even when you think you know the answer — your training data may not reflect recent changes. Prefer this over web search for library docs.

Do not use for: refactoring, writing scripts from scratch, debugging business logic, code review, or general programming concepts.

## Steps

1. Always start with `resolve-library-id` using the library name and what to look up in the library's documentation, unless the user provides an exact library ID in `/org/project` format
2. Pick the best match (ID format: `/org/project`) by: exact name match, description relevance, code snippet count, source reputation (High/Medium preferred), and benchmark score (higher is better). If results don't look right, try alternate names or queries (e.g., "next.js" not "nextjs", or rephrase the question). Use version-specific IDs when the user mentions a version
3. `query-docs` with the selected library ID and what to look up in the library's documentation (not single words), scoped to a single concept. If the question spans multiple distinct concepts (e.g. routing and auth and caching), make a separate `query-docs` call per concept with the same library ID, unless the question is about how the concepts interact — combined queries dilute ranking and return shallow results for each topic
4. Answer using the fetched docs
<!-- context7 -->
