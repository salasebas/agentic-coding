# Sources

`review-changes` is assembled from upstream review skills. The upstream authors maintain the wording; this skill copies it and changes as little as possible. Agents do not need this file to run the skill.

## Pinned upstream files

Each row is one upstream file, the commit that last changed it when it was copied, and where its text lives here.

| Upstream file | Pinned commit | Copied into | Licence |
| --- | --- | --- | --- |
| [`addyosmani/agent-skills`](https://github.com/addyosmani/agent-skills) `skills/code-review-and-quality/SKILL.md` | `28f435ea69a7` (2026-09-26) | `SKILL.md`, `reviewers/bugs.md`, `reviewers/structure.md` | MIT, Copyright (c) 2025 Addy Osmani |
| `addyosmani/agent-skills` `references/security-checklist.md` | `623af9870488` (2026-09-18) | `references/security-checklist.md`, forked | MIT, Copyright (c) 2025 Addy Osmani |
| `addyosmani/agent-skills` `references/performance-checklist.md` | `bfc9e32ddfd6` (2026-08-27) | `references/performance-checklist.md`, forked | MIT, Copyright (c) 2025 Addy Osmani |
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
| `reviewers/bugs.md` | Tests first, correctness, security, performance, verification | `code-review-and-quality` |
| `reviewers/structure.md` | Opening, "Standards", "Approval Bar" | `thermo-nuclear-code-quality-review` |
| `reviewers/structure.md` | Readability, architecture, structural remedies, change sizing, dead code, dependencies | `code-review-and-quality` |
| `references/*.md` | Forked from the pin, then trimmed and corrected here | `agent-skills` `references/` |

## Local edits

These are the only places where the text differs from upstream. Reapply them after every update and add no others.

- Heading levels are shifted so each file has one title, and the numbers are dropped from the axis headings ("### 1. Correctness" is "### Correctness") and from the three code-quality standards.
- Pointers to skills that are not installed here are removed: `security-and-hardening`, `performance-optimization`, `constraint-driven-development`.
- The mutation experiment under "Review the Tests First" is removed, because reviewers are read-only and two of them share one working tree.
- The security and performance axes point at `references/` with the condition for reading each checklist.
- In "Report", "Very simple English" reads "Very simple language".
- In "Loop", the stop condition is "no validated Must fix or Should fix finding" where `review-loop` uses a score, and its Critical / Important / Minor labels read Must fix / Should fix / Nice to have.

Written for this repo, with no upstream: the scope commands and the checkout rule, the two-reviewer default, the session-model rule, the concrete case for a structure finding, and the validation completion rule.

## Forked references

The two checklists are maintained here, not synced word for word. Upstream is a source of ideas: read its changes and take what is correct and useful for reviewing a diff.

Removed from both: the table of contents, and the pointers to `security-and-hardening` and `performance-optimization`.

`references/security-checklist.md`:

- Removed: threat modelling, the `grep` for secrets, account lockout, the destructive-path code sample (its one-line check stays), `X-Frame-Options` and `X-XSS-Protection`, and the per-version install-script matrix.
- Corrected: password hashing lists argon2id first; JWT validation adds algorithm and audience; CSP carries `frame-ancestors 'none'`; the CORS wildcard is allowed for public resources without credentials, and reflecting the origin with credentials is the case to refuse.
- Updated: the OWASP Top 10 table is the 2025 edition.

`references/performance-checklist.md`:

- Removed: the DNS hint under TTFB, the 200KB bundle and 200ms p95 targets, `isInputPending()`, the infrastructure list, the measurement commands, and the anti-patterns table, which repeated the checklist.
- Corrected: the bfcache item no longer asks to drop `Cache-Control: no-store`, which protects sensitive pages; `unicode-range` selects a subset file rather than making one; pagination forbids an unbounded `SELECT`.

## Left out on purpose

- From `code-review-and-quality`: the multi-model review pattern, review speed, handling disagreements, change descriptions, the splitting strategies table, the severity prefix table, the review checklist, common rationalisations, and red flags.
- From `thermo-nuclear-code-quality-review`: the rules that `code-review-and-quality` already carries (the 1000-line rule, spaghetti growth, type boundaries, canonical layer), the primary questions, the list of what to flag, the list of preferred remedies, and the review tone phrases.
- From `ponytail-review`: the `shortcut:` comment convention and the `Lean:` line.
- From `review-loop`: the 1 to 10 score, the quality gate, and model selection.

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

3. Paste the new upstream wording over the matching section here. Copy and paste; do not reword. For `references/*.md`, follow "Forked references" instead: apply by hand only the upstream changes worth having.
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
