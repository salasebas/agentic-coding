# Skills

My setup for coding agents: the global rules, the skills I keep, and the skills I install from other people.

## Skills kept here

| Skill | What it does |
| --- | --- |
| [`review-changes`](./skills/review-changes/SKILL.md) | Reviews a change for bugs and structure and validates every finding before reporting it. `loop` fixes and reviews again until nothing is left. |
| [`python-development`](./skills/python-development/SKILL.md) | Conventions for Python projects: `uv`, type hints, PEP 723 scripts. |
| [`typescript-development`](./skills/typescript-development/SKILL.md) | Conventions for TypeScript and JavaScript projects: strict mode, `pnpm`. |
| [`rust-development`](./skills/rust-development/SKILL.md) | Conventions for Rust projects: workspaces, crate features, public APIs. |
| [`golang-development`](./skills/golang-development/SKILL.md) | Conventions for Go projects, with a testing reference. |

The language skills load only in a project that uses that language. They are copies of sammcj's skills; [`skills/THIRD_PARTY.md`](./skills/THIRD_PARTY.md) has the attribution.

`review-changes` is assembled from other review skills. [`SOURCES.md`](./skills/review-changes/SOURCES.md) records where each part came from, why the skill is shaped the way it is, and how to update it.

### Install

One skill, or all of them:

```bash
npx skills add salasebas/skills -g -s review-changes
```

```bash
npx skills add salasebas/skills -g -s review-changes -s python-development -s typescript-development -s rust-development -s golang-development
```

As a plugin:

| Harness | Command |
| --- | --- |
| Claude Code | `claude plugin marketplace add salasebas/skills`, then `claude plugin install salasebas-skills@salasebas` |
| Codex | `codex plugin marketplace add salasebas/skills`, then `codex plugin add salasebas-skills` |

## Global rules

[`rules/AGENTS.md`](./rules/AGENTS.md) holds the instructions every session loads. Each harness reads it from a different place:

| Harness | Where the file goes |
| --- | --- |
| Codex, Grok | `~/.agents/AGENTS.md` |
| Claude Code | `~/.claude/CLAUDE.md`, a symlink to `~/.agents/AGENTS.md` |
| Cursor | `~/.cursor/rules/agents.mdc`, a symlink to `~/.agents/AGENTS.md`. The frontmatter at the top of the file is for Cursor. |

Rules for one external tool live in [`rules/integrations/`](./rules/integrations/), so they install separately:

| Rule | Where the file goes |
| --- | --- |
| [`context7.md`](./rules/integrations/context7.md) | `~/.claude/rules/context7.md` for Claude Code, `~/.cursor/rules/context7.mdc` for Cursor. Codex and Grok read only `AGENTS.md`, so append the block there. |

## Skills installed from other people

These are not copied here. Install any of them with `npx skills add <source> -g -s <skill>`.

| Source | Skills |
| --- | --- |
| [`mattpocock/skills`](https://github.com/mattpocock/skills) | `ask-matt`, `code-review`, `codebase-design`, `diagnosing-bugs`, `domain-modeling`, `grill-me`, `grill-with-docs`, `grilling`, `handoff`, `implement`, `implement-spec`, `improve-codebase-architecture`, `pr`, `research`, `retro`, `setup-matt-pocock-skills`, `tdd`, `teach`, `to-spec`, `to-tickets`, `triage`, `wait-what`, `wayfinder`, `writing-for-agents` |
| [`emilkowalski/skills`](https://github.com/emilkowalski/skills) | `animation-vocabulary`, `emil-design-eng`, `pick-ui-library`, `review-animations` |
| [`sammcj/agentic-coding`](https://github.com/sammcj/agentic-coding) | `shell-scripting`, `writing-documentation-with-diataxis` |
| [`cursor/plugins`](https://github.com/cursor/plugins), `pstack` | `technical-writing`, `unslop` |
| `cursor/plugins`, `cursor-team-kit` | `deslop` |
| [`Leonxlnx/taste-skill`](https://github.com/Leonxlnx/taste-skill) | `design-taste-frontend` |
| [`vercel-labs/agent-browser`](https://github.com/vercel-labs/agent-browser) | `agent-browser` |
| [`vercel-labs/skills`](https://github.com/vercel-labs/skills) | `find-skills` |

For the two `cursor/plugins` rows the source is the folder URL, for example `https://github.com/cursor/plugins/tree/main/pstack`.

[`sources.json`](./sources.json) records the folder hash of each copy I last installed, so a rename or removal upstream shows up. It is not a backup of the skill text.

## Licence

[MIT](./LICENSE). `review-changes` includes text from other MIT-licensed skills, named in its [`SOURCES.md`](./skills/review-changes/SOURCES.md). The skills in [`skills/THIRD_PARTY.md`](./skills/THIRD_PARTY.md) keep their own licence.
