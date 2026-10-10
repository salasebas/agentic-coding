# Sources

`review-changes` is assembled from upstream review skills. The upstream authors maintain the wording; this skill copies it and changes as little as possible. Agents do not need this file to run the skill.

## Pinned upstream files

Each row is one upstream file, the commit that last changed it when it was copied, and where its text lives here.

| Upstream file | Pinned commit | Copied into | Licence |
| --- | --- | --- | --- |
| [`addyosmani/agent-skills`](https://github.com/addyosmani/agent-skills) `skills/code-review-and-quality/SKILL.md` | `28f435ea69a7` (2026-09-26) | `SKILL.md`, `reviewers/structure.md` | MIT, Copyright (c) 2025 Addy Osmani |
| [`cursor/plugins`](https://github.com/cursor/plugins) `thermos/skills/thermos/SKILL.md` | `5080d38e4c52` (2026-05-28) | `SKILL.md` | MIT, Copyright (c) 2026 Cursor |
| `cursor/plugins` `thermos/skills/thermo-nuclear-review/SKILL.md` | `5102244dabd6` (2026-05-28) | `reviewers/bugs.md` | MIT, Copyright (c) 2026 Cursor |
| `cursor/plugins` `thermos/skills/thermo-nuclear-code-quality-review/SKILL.md` | `5102244dabd6` (2026-05-28) | `reviewers/structure.md` | MIT, Copyright (c) 2026 Cursor |
| [`DietrichGebert/ponytail`](https://github.com/DietrichGebert/ponytail) `skills/ponytail-review/SKILL.md` | `2a1fe842d078` (2026-10-08) | `SKILL.md` | MIT, Copyright (c) 2026 DietrichGebert |
| [`2dmurali/review-loop-skill`](https://github.com/2dmurali/review-loop-skill) `review-loop/SKILL.md` | `dec43b15b0e3` (2026-05-08) | `SKILL.md` | MIT, as stated in its `SKILL.md`; the repository has no licence file |

## What each file takes

| File here | Section | Upstream |
| --- | --- | --- |
| `SKILL.md` | Approval standard, "Lead with what matters", "Honesty in Review" | `code-review-and-quality` |
| `SKILL.md` | Workflow steps 1 to 5 | `thermos` |
| `SKILL.md` | Opening line, scope default, "Validate" bullets, "Report" format | `ponytail-review` |
| `SKILL.md` | "Loop" defaults, stop conditions, revision and memory rules | `review-loop` |
| `reviewers/bugs.md` | Prompt, scope, guidelines, final response, critical rules | `thermo-nuclear-review`, whole |
| `reviewers/structure.md` | Opening, "Standards", "Approval Bar" | `thermo-nuclear-code-quality-review` |
| `reviewers/structure.md` | Structural remedies | `code-review-and-quality` |

## Local edits

These are the only places where the text differs from upstream. Reapply them after every update and add no others.

- Heading levels are shifted so each file has one title, and the numbers are dropped from the three code-quality standards.
- In "Report", "Very simple English" reads "Very simple language".
- In "Loop", the stop condition is "no validated Must fix or Should fix finding" where `review-loop` uses a score, and its Critical / Important / Minor labels read Must fix / Should fix / Nice to have.

Written for this repo, with no upstream: the scope commands and the checkout rule, the two-reviewer default, the session-model rule, the concrete case for a structure finding, the validation completion rule, and the one-line "Coverage" section of the bugs reviewer.

## Left out on purpose

- From `code-review-and-quality`: the five review axes and their question lists, tests first, verify the verification, change sizing, dead code hygiene, dependency discipline, the multi-model review pattern, review speed, handling disagreements, change descriptions, the severity prefix table, the review checklist, common rationalisations, red flags, and the `references/` checklists. See "Why the reviewers are short".
- From `thermo-nuclear-code-quality-review`: the primary questions, the list of what to flag, the list of preferred remedies, and the review tone phrases. All eight of its standards are kept.
- From `ponytail-review`: the `shortcut:` comment convention and the `Lean:` line.
- From `review-loop`: the 1 to 10 score, the quality gate, and model selection.

## Why the reviewers are short

The question lists and checklists were removed after a comparison on three merged pull requests (two in `saml-rs`, one in `better-auth-rb`), run on 2026-10-10 with one reviewer per variant and no repeats.

| Reviewer | With lists and checklists | Short |
| --- | --- | --- |
| Bugs, findings confirmed against the code | 3 of 5 | 7 of 7 |
| Structure, findings confirmed (one pull request) | 2 of 3 | 3 of 3 |
| Tokens per bugs review, mean | 95k | 91k |

Both variants found the two shared defects. Only the short one found the most severe defect of the run. The sample is small, so read this as "the lists did not help", not as "short is better". Before adding a list back, run the same comparison. The structure reviewer tested carried three of the eight code-quality standards; the other five were restored afterwards and have not been compared.

## How to update from upstream

1. For each row of "Pinned upstream files", get the commit that last changed the file:

   ```bash
   gh api "repos/<owner>/<repo>/commits?path=<path>&per_page=1" --jq '.[0].sha'
   ```

   If it starts with the pinned commit, that file has not changed; skip it.
2. For a file that changed, read what changed since the pin:

   ```bash
   gh api "repos/<owner>/<repo>/compare/<pinned>...<new>" --jq '.files[] | select(.filename == "<path>") | .patch'
   ```

3. Paste the new upstream wording over the matching section here. Copy and paste; do not reword.
4. Reapply only the edits listed under "Local edits". If upstream added a section, decide whether it belongs here or under "Left out on purpose", and record the decision in this file.
5. Update the pinned commit and date in the table.
6. Bump `version` in `.claude-plugin/plugin.json`.

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
