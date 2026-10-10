# Sources

`review-changes` is assembled from upstream review skills. The text is copied with as few edits as possible; this file records what came from where. Agents do not need this file to run the skill.

Upstream copies were read on 2026-10-10.

| Part of this skill | Upstream | Licence |
| --- | --- | --- |
| `SKILL.md`: approval standard, "Lead with what matters", "Honesty in Review" | [`addyosmani/agent-skills` `code-review-and-quality`](https://github.com/addyosmani/agent-skills/blob/main/skills/code-review-and-quality/SKILL.md) | MIT, Copyright (c) 2025 Addy Osmani |
| `SKILL.md`: workflow steps 1 to 5 (two parallel reviewers, then synthesis) | [`cursor/plugins` `thermos`](https://github.com/cursor/plugins/blob/main/thermos/skills/thermos/SKILL.md) | MIT, Copyright (c) 2026 Cursor |
| `SKILL.md`: opening line, scope default, "Validate" bullets, "Report" format | [`DietrichGebert/ponytail` `ponytail-review`](https://github.com/DietrichGebert/ponytail/blob/main/skills/ponytail-review/SKILL.md) | MIT, Copyright (c) 2026 DietrichGebert |
| `SKILL.md`: "Loop" defaults, stop conditions, revision and memory rules | [`2dmurali/review-loop-skill` `review-loop`](https://github.com/2dmurali/review-loop-skill/blob/main/review-loop/SKILL.md) | MIT, as stated in its `SKILL.md`; the repository has no licence file |
| `reviewers/bugs.md`: prompt, scope, guidelines, final response, critical rules | [`cursor/plugins` `thermo-nuclear-review`](https://github.com/cursor/plugins/blob/main/thermos/skills/thermo-nuclear-review/SKILL.md) | MIT, Copyright (c) 2026 Cursor |
| `reviewers/bugs.md`: tests first, correctness, security, performance, verification | `addyosmani/agent-skills` `code-review-and-quality` | MIT, Copyright (c) 2025 Addy Osmani |
| `reviewers/structure.md`: opening, "Standards", "Approval Bar" | [`cursor/plugins` `thermo-nuclear-code-quality-review`](https://github.com/cursor/plugins/blob/main/thermos/skills/thermo-nuclear-code-quality-review/SKILL.md) | MIT, Copyright (c) 2026 Cursor |
| `reviewers/structure.md`: readability, architecture, structural remedies, change sizing, dead code, dependencies | `addyosmani/agent-skills` `code-review-and-quality` | MIT, Copyright (c) 2025 Addy Osmani |

The scope commands, the two-reviewer default, the session-model rule, and the validation completion rule are written for this repo.

## Left out on purpose

- From `code-review-and-quality`: the multi-model review pattern, review speed, handling disagreements, change descriptions, the splitting strategies table, the review checklist, common rationalisations, red flags, and the pointers to skills that are not installed here.
- From `thermo-nuclear-code-quality-review`: the rules that `code-review-and-quality` already carries (the 1000-line rule, spaghetti growth, type boundaries, canonical layer), the list of preferred remedies, and the review tone phrases.
- From `ponytail-review`: the `shortcut:` comment convention and the `Lean:` line.
- From `review-loop`: the 1 to 10 score, the quality gate, and model selection.

## MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
