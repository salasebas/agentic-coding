# How I work

This is the set of agent skills I keep installed, and when I reach for each one.

I do not author these skills, and I do not copy them into this repo. Upstream is the source of truth. Each entry links to the skill and gives the command that installs it again. [`sources.json`](./sources.json) records the folder hash of the copy I last installed, so a rename or deletion upstream is visible. It is not a backup of the skill text.

The skills I do publish live in [salasebas/skills](https://github.com/salasebas/skills): `orchestrate`, `ship`, and `improve`.

## A change, end to end

Engineering work goes through [Matt Pocock's skills](https://github.com/mattpocock/skills). Once per repo, run `setup-matt-pocock-skills`. It records the issue tracker, triage labels, and where `GLOSSARY.md` and ADRs live.

1. **Shape the change.** `ask-matt` if it is not obvious which skill fits. `grill-with-docs` when the plan should also settle terms and decisions. `grill-me` when there is nothing to write down yet.
2. **Write it down.** `to-spec` turns the conversation into a spec. `to-tickets` breaks that spec into tracer tickets. `wayfinder` is the same idea when the work is bigger than one session.
3. **Build it.** `implement` for one slice. `implement-spec` when the whole spec should land on one integration branch. Both drive `tdd`. `prototype` is for a throwaway check before that, not for the real change.
4. **Close it.** `code-review` on the diff, then `pr` for the pull request body. `retro` after a session that should change how the agent works next time.

`triage` moves issues through that system. `diagnosing-bugs` is the loop for something broken or slow. `improve-codebase-architecture` is a periodic survey, not part of a single change.

Install the whole engineering and productivity set:

```bash
npx skills add mattpocock/skills -g -y --skill \
  ask-matt codebase-design code-review diagnosing-bugs domain-modeling \
  grill-with-docs implement implement-spec improve-codebase-architecture \
  pr prototype research retro setup-matt-pocock-skills tdd to-spec \
  to-tickets triage wayfinder wizard grill-me grilling handoff teach \
  to-questionnaire wait-what writing-for-agents
```

## Engineering

| Skill | When I use it | Upstream | Install |
| --- | --- | --- | --- |
| `ask-matt` | I know the outcome and not which skill | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/ask-matt/SKILL.md) | `npx skills add mattpocock/skills -g -s ask-matt` |
| `setup-matt-pocock-skills` | First time in a repo | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/setup-matt-pocock-skills/SKILL.md) | `npx skills add mattpocock/skills -g -s setup-matt-pocock-skills` |
| `grill-with-docs` | Sharpen a plan and update the glossary and ADRs as we go | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/grill-with-docs/SKILL.md) | `npx skills add mattpocock/skills -g -s grill-with-docs` |
| `grill-me` | Same interview, no docs | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/productivity/grill-me/SKILL.md) | `npx skills add mattpocock/skills -g -s grill-me` |
| `grilling` | The interview primitive the other skills call | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/productivity/grilling/SKILL.md) | `npx skills add mattpocock/skills -g -s grilling` |
| `domain-modeling` | Naming a term or writing an ADR | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/domain-modeling/SKILL.md) | `npx skills add mattpocock/skills -g -s domain-modeling` |
| `codebase-design` | Designing a module interface | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/codebase-design/SKILL.md) | `npx skills add mattpocock/skills -g -s codebase-design` |
| `to-spec` | The conversation is already the spec | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/to-spec/SKILL.md) | `npx skills add mattpocock/skills -g -s to-spec` |
| `to-tickets` | Break a spec into blocking tickets | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/to-tickets/SKILL.md) | `npx skills add mattpocock/skills -g -s to-tickets` |
| `wayfinder` | The work does not fit in one session | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/wayfinder/SKILL.md) | `npx skills add mattpocock/skills -g -s wayfinder` |
| `implement` | Build one slice from a spec or tickets | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/implement/SKILL.md) | `npx skills add mattpocock/skills -g -s implement` |
| `implement-spec` | Build the whole spec on one integration branch | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/implement-spec/SKILL.md) | `npx skills add mattpocock/skills -g -s implement-spec` |
| `tdd` | Red, green, refactor | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/tdd/SKILL.md) | `npx skills add mattpocock/skills -g -s tdd` |
| `prototype` | Check a state model or a UI before building it | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/prototype/SKILL.md) | `npx skills add mattpocock/skills -g -s prototype` |
| `diagnosing-bugs` | Something is broken, failing, or slow | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/diagnosing-bugs/SKILL.md) | `npx skills add mattpocock/skills -g -s diagnosing-bugs` |
| `research` | A question that needs primary sources | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/research/SKILL.md) | `npx skills add mattpocock/skills -g -s research` |
| `improve-codebase-architecture` | Look for deepening opportunities | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/improve-codebase-architecture/SKILL.md) | `npx skills add mattpocock/skills -g -s improve-codebase-architecture` |
| `triage` | Move an issue until an agent can take it | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/triage/SKILL.md) | `npx skills add mattpocock/skills -g -s triage` |
| `code-review` | Review a branch against standards and the spec | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/code-review/SKILL.md) | `npx skills add mattpocock/skills -g -s code-review` |
| `pr` | Write the pull request body | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/pr/SKILL.md) | `npx skills add mattpocock/skills -g -s pr` |
| `retro` | After a session, fix the agent's environment | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/retro/SKILL.md) | `npx skills add mattpocock/skills -g -s retro` |
| `wizard` | A human has to click through a dashboard or paste a secret | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/engineering/wizard/SKILL.md) | `npx skills add mattpocock/skills -g -s wizard` |
| `handoff` | Another session should continue this one | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/productivity/handoff/SKILL.md) | `npx skills add mattpocock/skills -g -s handoff` |
| `wait-what` | The last message did not land | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/productivity/wait-what/SKILL.md) | `npx skills add mattpocock/skills -g -s wait-what` |
| `to-questionnaire` | Someone else has to answer the open decision | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/productivity/to-questionnaire/SKILL.md) | `npx skills add mattpocock/skills -g -s to-questionnaire` |
| `teach` | Learn a concept inside this workspace | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/productivity/teach/SKILL.md) | `npx skills add mattpocock/skills -g -s teach` |
| `writing-for-agents` | Edit a skill, `AGENTS.md`, or `CLAUDE.md` | [SKILL.md](https://github.com/mattpocock/skills/blob/main/skills/productivity/writing-for-agents/SKILL.md) | `npx skills add mattpocock/skills -g -s writing-for-agents` |

## Interface

| Skill | When I use it | Upstream | Install |
| --- | --- | --- | --- |
| `emil-design-eng` | Polish, motion, and the details that make a UI feel finished | [SKILL.md](https://github.com/emilkowalski/skills/blob/main/skills/emil-design-eng/SKILL.md) | `npx skills add emilkowalski/skills -g -s emil-design-eng` |
| `animation-vocabulary` | Name a motion effect precisely | [SKILL.md](https://github.com/emilkowalski/skills/blob/main/skills/animation-vocabulary/SKILL.md) | `npx skills add emilkowalski/skills -g -s animation-vocabulary` |
| `review-animations` | Review motion against that craft bar | [SKILL.md](https://github.com/emilkowalski/skills/blob/main/skills/review-animations/SKILL.md) | `npx skills add emilkowalski/skills -g -s review-animations` |
| `pick-ui-library` | Choose a library for a specific UI job | [SKILL.md](https://github.com/emilkowalski/skills/blob/main/skills/pick-ui-library/SKILL.md) | `npx skills add emilkowalski/skills -g -s pick-ui-library` |
| `design-taste-frontend` | A page that should not look templated | [SKILL.md](https://github.com/Leonxlnx/taste-skill/blob/main/skills/taste-skill/SKILL.md) | `npx skills add Leonxlnx/taste-skill -g -s design-taste-frontend` |
| `make-interfaces-feel-better` | Hover, shadow, border, and easing on a component | [SKILL.md](https://github.com/jakubkrehel/make-interfaces-feel-better/blob/main/skills/make-interfaces-feel-better/SKILL.md) | `npx skills add jakubkrehel/make-interfaces-feel-better -g -s make-interfaces-feel-better` |

## Also installed

These are on the machine. They are not part of the loop above.

| Skill | Source | Install |
| --- | --- | --- |
| `agent-browser` | [vercel-labs/agent-browser](https://github.com/vercel-labs/agent-browser) | `npx skills add vercel-labs/agent-browser -g -s agent-browser` |
| `code-review-and-quality` | [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) | `npx skills add addyosmani/agent-skills -g -s code-review-and-quality` |
| `find-skills` | [vercel-labs/skills](https://github.com/vercel-labs/skills) | `npx skills add vercel-labs/skills -g -s find-skills` |
| `git-commit` | [github/awesome-copilot](https://github.com/github/awesome-copilot) | `npx skills add github/awesome-copilot -g -s git-commit` |
| `receiving-code-review` | [obra/superpowers](https://github.com/obra/superpowers) | `npx skills add obra/superpowers -g -s receiving-code-review` |
| `skill-creator` | [anthropics/skills](https://github.com/anthropics/skills) | `npx skills add anthropics/skills -g -s skill-creator` |
| `vercel-composition-patterns` | [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills) | `npx skills add vercel-labs/agent-skills -g -s vercel-composition-patterns` |
| `writing-documentation-with-diataxis` | [sammcj/agentic-coding](https://github.com/sammcj/agentic-coding) | `npx skills add sammcj/agentic-coding -g -s writing-documentation-with-diataxis` |

## Skills I publish

[salasebas/skills](https://github.com/salasebas/skills)

| Skill | When I use it |
| --- | --- |
| `orchestrate` | A task that needs discovery, one executor, and a separate review |
| `ship` | Implement from a plan, or make the plan and then implement it |
| `improve` | A read-only survey that writes plans for another agent |

```bash
npx skills add salasebas/skills
```
