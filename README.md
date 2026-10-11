# Skills

This is how I work with coding agents: my global instructions in [`rules/`](./rules/), my own skills in [`skills/`](./skills/), and the skills I keep installed.

[`rules/AGENTS.md`](./rules/AGENTS.md) is my file. The installed copy lives at `~/.agents/AGENTS.md`. [`rules/CLAUDE.md`](./rules/CLAUDE.md) is a symlink to it, for the tools that read `CLAUDE.md`. The frontmatter at the top of `AGENTS.md` is for Cursor, which reads the file as an always-applied rule through `~/.cursor/rules/agents.mdc`.

Rules for external tools live in [`rules/integrations/`](./rules/integrations/), one file per tool, so they can be installed or left out without touching `AGENTS.md`.

| Rule | For | Install as |
| --- | --- | --- |
| [`context7.md`](./rules/integrations/context7.md) | The Context7 MCP server | `~/.claude/rules/context7.md` for Claude Code, `~/.cursor/rules/context7.mdc` for Cursor. Codex and Grok read only `AGENTS.md`: append the file there if you use Context7 with them. |

## Mine

| Skill | Source | Install |
| --- | --- | --- |
| `review-changes` | [SKILL.md](./skills/review-changes/SKILL.md) | `npx skills add salasebas/skills -g -s review-changes` |

`review-changes` is assembled from upstream review skills. [`SOURCES.md`](./skills/review-changes/SOURCES.md) records which part came from which skill.

To update `review-changes`, follow [How to update from upstream](./skills/review-changes/SOURCES.md#how-to-update-from-upstream): paste the new upstream wording and reapply only the local edits listed in that file.

The repo is also a plugin marketplace named `salasebas`, with one plugin, `salasebas-skills`. The plugin version lives in [`.claude-plugin/plugin.json`](./.claude-plugin/plugin.json).

| Harness | Install |
| --- | --- |
| Claude Code | `claude plugin marketplace add salasebas/skills`, then `claude plugin install salasebas-skills@salasebas` |
| Codex | `codex plugin marketplace add salasebas/skills`, then `codex plugin add salasebas-skills` |

I do not author the skills below, and I do not copy them into this repo. Upstream is the source of truth. Each entry links to the skill and gives the command that installs it again. [`sources.json`](./sources.json) records the folder hash of the copy I last installed, so a rename or deletion upstream is visible. It is not a backup of the skill text.

## Engineering

| Skill | Upstream | Install |
| --- | --- | --- |
| `ask-matt` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/ask-matt/SKILL.md) | `npx skills add mattpocock/skills -g -s ask-matt` |
| `setup-matt-pocock-skills` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/setup-matt-pocock-skills/SKILL.md) | `npx skills add mattpocock/skills -g -s setup-matt-pocock-skills` |
| `grill-with-docs` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/grill-with-docs/SKILL.md) | `npx skills add mattpocock/skills -g -s grill-with-docs` |
| `grill-me` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/productivity/grill-me/SKILL.md) | `npx skills add mattpocock/skills -g -s grill-me` |
| `grilling` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/productivity/grilling/SKILL.md) | `npx skills add mattpocock/skills -g -s grilling` |
| `domain-modeling` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/domain-modeling/SKILL.md) | `npx skills add mattpocock/skills -g -s domain-modeling` |
| `codebase-design` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/codebase-design/SKILL.md) | `npx skills add mattpocock/skills -g -s codebase-design` |
| `to-spec` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/to-spec/SKILL.md) | `npx skills add mattpocock/skills -g -s to-spec` |
| `to-tickets` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/to-tickets/SKILL.md) | `npx skills add mattpocock/skills -g -s to-tickets` |
| `wayfinder` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/wayfinder/SKILL.md) | `npx skills add mattpocock/skills -g -s wayfinder` |
| `implement` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/implement/SKILL.md) | `npx skills add mattpocock/skills -g -s implement` |
| `implement-spec` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/implement-spec/SKILL.md) | `npx skills add mattpocock/skills -g -s implement-spec` |
| `tdd` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/tdd/SKILL.md) | `npx skills add mattpocock/skills -g -s tdd` |
| `prototype` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/prototype/SKILL.md) | `npx skills add mattpocock/skills -g -s prototype` |
| `diagnosing-bugs` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/diagnosing-bugs/SKILL.md) | `npx skills add mattpocock/skills -g -s diagnosing-bugs` |
| `research` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/research/SKILL.md) | `npx skills add mattpocock/skills -g -s research` |
| `improve-codebase-architecture` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/improve-codebase-architecture/SKILL.md) | `npx skills add mattpocock/skills -g -s improve-codebase-architecture` |
| `triage` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/triage/SKILL.md) | `npx skills add mattpocock/skills -g -s triage` |
| `code-review` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/code-review/SKILL.md) | `npx skills add mattpocock/skills -g -s code-review` |
| `pr` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/pr/SKILL.md) | `npx skills add mattpocock/skills -g -s pr` |
| `retro` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/retro/SKILL.md) | `npx skills add mattpocock/skills -g -s retro` |
| `wizard` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/wizard/SKILL.md) | `npx skills add mattpocock/skills -g -s wizard` |
| `handoff` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/productivity/handoff/SKILL.md) | `npx skills add mattpocock/skills -g -s handoff` |
| `wait-what` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/productivity/wait-what/SKILL.md) | `npx skills add mattpocock/skills -g -s wait-what` |
| `to-questionnaire` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/productivity/to-questionnaire/SKILL.md) | `npx skills add mattpocock/skills -g -s to-questionnaire` |
| `teach` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/productivity/teach/SKILL.md) | `npx skills add mattpocock/skills -g -s teach` |
| `writing-for-agents` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/productivity/writing-for-agents/SKILL.md) | `npx skills add mattpocock/skills -g -s writing-for-agents` |

## Interface

| Skill | Upstream | Install |
| --- | --- | --- |
| `emil-design-eng` | [SKILL.md](https://github.com/emilkowalski/skills/blob/main/skills/emil-design-eng/SKILL.md) | `npx skills add emilkowalski/skills -g -s emil-design-eng` |
| `animation-vocabulary` | [SKILL.md](https://github.com/emilkowalski/skills/blob/main/skills/animation-vocabulary/SKILL.md) | `npx skills add emilkowalski/skills -g -s animation-vocabulary` |
| `review-animations` | [SKILL.md](https://github.com/emilkowalski/skills/blob/main/skills/review-animations/SKILL.md) | `npx skills add emilkowalski/skills -g -s review-animations` |
| `pick-ui-library` | [SKILL.md](https://github.com/emilkowalski/skills/blob/main/skills/pick-ui-library/SKILL.md) | `npx skills add emilkowalski/skills -g -s pick-ui-library` |
| `design-taste-frontend` | [SKILL.md](https://github.com/Leonxlnx/taste-skill/blob/main/skills/taste-skill/SKILL.md) | `npx skills add Leonxlnx/taste-skill -g -s design-taste-frontend` |

## Also installed

| Skill | Source | Install |
| --- | --- | --- |
| `agent-browser` | [vercel-labs/agent-browser](https://github.com/vercel-labs/agent-browser) | `npx skills add vercel-labs/agent-browser -g -s agent-browser` |
| `find-skills` | [vercel-labs/skills](https://github.com/vercel-labs/skills) | `npx skills add vercel-labs/skills -g -s find-skills` |
| `unslop` | [cursor/plugins](https://github.com/cursor/plugins/tree/main/pstack) | `npx skills add https://github.com/cursor/plugins/tree/main/pstack -g -s unslop` |
| `technical-writing` | [cursor/plugins](https://github.com/cursor/plugins/tree/main/pstack) | `npx skills add https://github.com/cursor/plugins/tree/main/pstack -g -s technical-writing` |
| `deslop` | [cursor/plugins](https://github.com/cursor/plugins/tree/main/cursor-team-kit) | `npx skills add https://github.com/cursor/plugins/tree/main/cursor-team-kit -g -s deslop` |
| `git-commit` | [github/awesome-copilot](https://github.com/github/awesome-copilot) | `npx skills add github/awesome-copilot -g -s git-commit` |
| `writing-documentation-with-diataxis` | [sammcj/agentic-coding](https://github.com/sammcj/agentic-coding) | `npx skills add sammcj/agentic-coding -g -s writing-documentation-with-diataxis` |

## License

[MIT](./LICENSE). `review-changes` includes text from other MIT-licensed skills; [`SOURCES.md`](./skills/review-changes/SOURCES.md) names them.
