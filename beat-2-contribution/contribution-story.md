# Contribution: Align date error message with field input format

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Rocio Dominguez Vidana
**GitHub Username:** rdv-chio
**Issue:** [https://github.com/HHS/simpler-grants-gov/issues/6192](https://github.com/HHS/simpler-grants-gov/issues/6192)
**Pull Request:** [https://github.com/HHS/simpler-grants-gov/pull/13119](https://github.com/HHS/simpler-grants-gov/pull/13119)

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Automated GitHub issue scouting and multi-stage funnel filtering**

I upgraded `issue-select` into `issue-scout`, creating search queries for Python CLI bugs and a ranking pipeline that drops assigned or stale issues. During the live scan, ranking by fewest comments proved to be a no-op because the top 12 results all had zero comments, forcing the tie-breaker (`updatedAt`) to do all the work. Additionally, `repo_active` accepted brand-new, zero-star repos created on the scan date. To address this, I adjusted my strategy to explicitly filter for non-docs bug labels and plan to add a minimum repository star or release threshold to prevent toy projects from passing.

**Unit 6: Outgoing comment enforcement and scoped repository orientation**

I integrated `voice-guard` as a Claude Code `PreToolUse` hook, establishing a canonical upstream voice guide and blocking unauthorized or unverified outgoing comments. During testing, the doorway intercepted an initial claim draft that lacked an explicit timeframe, enforcing the rule before any public comment went out. I set up a localized `CLAUDE.local.md` file within `.git/info/exclude` to retain environment and testing conventions for `simpler-grants-gov` without polluting pull request diffs.

**Unit 7: Automated ship-gate verification and PR template standardization**

I configured `ship-manifest.md` to gate outgoing code against repository test suites and lint rules. When `ship-gate` flagged pre-existing failures on `main` within `withFeatureFlag.test.tsx` and `generalUtils.test.ts`, I isolated the active test check to `applyFormUtils.test.ts` and moved the full test run to the exclusions table with dated justifications. The gate ensured all PR template sections, issue linkage (`Fixes #6192`), and AI disclosures complied with upstream standards prior to opening the pull request.

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I searched for candidates matching my backend and data validation profile, prioritizing Python tasks with concrete input and output specifications. My scout's direct evaluation of `HHS/simpler-grants-gov#6192` yielded an `accept` verdict with a `moderate` difficulty rating, confirming that the repository is active, unarchived, and explicitly welcoming of AI-assisted contributions under its contributing policy. The issue targets an isolated formatting bug where validation error messages on date fields do not align with the expected `MM-DD-YYYY` pattern.

While candidate `dna#127` also passed the required rubric gates, it presented an active, unaccepted claim from another contributor and leaned toward frontend settings UI, conflicting with my personal profile. In contrast, `simpler-grants-gov#6192` had zero assignees, zero competing comments, and a clear acceptance criterion. The scout proved effective at catching conflicting active claims in competing issues, though it initially allowed brand-new repositories during broad scans before scope feasibility filtering was added.

Claim comment: <https://github.com/HHS/simpler-grants-gov/issues/6192#issuecomment-6379656108>

**Toolkit**

- Tool: issue-scout (direct grading mode with the scope-feasible rubric patch)
- What it produced: An `accept` verdict for `simpler-grants-gov#6192` with all required checks passing and a `moderate` difficulty rating.
- Difficulties: The scout's literal `issue_unclaimed` rule passed competing candidate `dna#127` despite an unanswered human claim in the thread, requiring manual inspection of comments.
- What changed: Applied the `scope-feasible` rubric patch to distinguish between bounded tasks and open design edges, using difficulty notes rather than rejecting bounded issues outright.

---

## Understanding the Issue

**Problem Description**

When a user enters an invalid or out-of-range date (such as `01/01/275760`), the backend validation engine returns an ISO 8601 validation error string. The frontend validation warning pipeline strips quotes from the error message but does not translate the ISO date pattern (`YYYY-MM-DD`) into the expected `MM-DD-YYYY` user format, producing confusing feedback that conflicts with the form field's helper text and placeholder.

**Expected Behavior**

Per the acceptance criteria of Issue #6192, validation error messages on date fields should read back using the `MM-DD-YYYY` format to match user input expectations and form instructions.

**Current Behavior**

The frontend renders the raw backend ISO error string (`"275760-01-01 is not a date"`), failing to provide format guidance or translate the date format into `MM-DD-YYYY`.

**Affected Components**

- `frontend/src/utils/applyForm/applyFormUtils.ts` (`formatValidationWarning` and `validationWarningOverrides`)
- `frontend/src/utils/applyForm/applyFormUtils.test.ts`
- Target date form fields across `OpportunityEditForm.tsx` and related grantor form schemas.

---

## Reproduction Process

**Environment Setup**

- Operating System: Linux (WSL2 / Ubuntu 22.04 on Windows)
- Node.js runtime: v22.23.3 (executed on v22.23.3; repository pins v24.19.0 in `frontend/.nvmrc`)
- Package manager: npm v10.9.2
- Target repository: `HHS/simpler-grants-gov`
- Commit SHA: `e6bb081e7739f9da7a1e35b3b6e7d94d5a2fcade`
- Working directory: `frontend/`

Challenges: The repository's `.nvmrc` specifies Node `v24.19.0`, which differed from the local `v22.23.3` runtime. Executing through `frontend/` with legacy peer dependency handling permitted consistent test execution, and the version mismatch was explicitly disclosed in the reproduction report.

**Steps to Reproduce**

1. Navigate to the frontend workspace: `cd frontend`
2. Create an isolated unit test in `repro.test.ts` passing the raw backend validation error string from Issue #6192 (`"'275760-01-01' is not a 'date'"`) to `formatValidationWarning("project_end_date", rawBackendError)`.
3. Run `npm test -- repro.test.ts` to assert against the expected `MM-DD-YYYY` format.
4. Observed result: Jest fails with `Expected pattern: /MM-DD-YYYY/i` and `Received string: "275760-01-01 is not a date"`.

**Reproduction Evidence**

- **Commit showing reproduction:** Verified on commit `e6bb081e7739f9da7a1e35b3b6e7d94d5a2fcade` (temporary test file `repro.test.ts` removed prior to branch commit).
- **Screenshots/logs:** Verbatim test run output from `npm test -- repro.test.ts` demonstrating received string `"275760-01-01 is not a date"`, and GitHub reproduction comment posted on thread: <https://github.com/HHS/simpler-grants-gov/issues/6192#issuecomment-6590808029>.
- **My findings:** The backend JSON schema validator emits full-date errors formatted in ISO standard. The frontend utility `validationWarningOverrides` in `applyFormUtils.ts` strips single quotes, but has no transformation rule for date patterns, passing the ISO string directly to the UI.

**Toolkit**

- Tool: repro-check (via Claude Code terminal execution)
- What it produced: An `accept` grade confirming verbatim failing output matching the acceptance criteria of #6192.
- Difficulties: Required multiple iterations because the grader rejected mock component assertions and mock form state runs that did not execute real validation pipelines.
- What changed: Extracted the transformation utility into an unmocked unit test asserting the exact string from the maintainer's screenshot.

---

## Solution Approach

**Analysis**

The bug originates in `frontend/src/utils/applyForm/applyFormUtils.ts`. When the backend schema validator rejects a date input, it returns an ISO error string such as `'YYYY-MM-DD' is not a 'date'`. In `validationWarningOverrides`, incoming error messages only undergo single-quote stripping and generic required-property replacement. Because no date-specific parsing or replacement is executed, the user sees raw ISO dates (e.g. `"275760-01-01 is not a date"`), conflicting with the form's `MM/DD/YYYY` helper guidance.

**Proposed Solution**

Standardize explicitly on `MM-DD-YYYY` format in date validation feedback to satisfy the issue's acceptance criteria. Update `validationWarningOverrides` in `frontend/src/utils/applyForm/applyFormUtils.ts` to detect date format error messages using regular expressions (`/'?(\d{4})-(\d{2})-(\d{2})'? is not a '?date'?/i`), reorder the date components into `$2-$3-$1` (`MM-DD-YYYY`), and instruct the user to use `MM-DD-YYYY` format. Also transform generic format errors (`must match format "date"`) into `${formattedTitle} must be a valid date in MM-DD-YYYY format.`.

Deliberately out of scope: No changes to backend schema validators, API endpoints, or non-date validation warning transforms.

**Implementation Plan**

1. Add unit test assertions in `frontend/src/utils/applyForm/applyFormUtils.test.ts` under `describe("formatValidationWarning")` verifying both specific ISO date error strings and generic date format warnings return messages specifying `MM-DD-YYYY`.
2. Update `validationWarningOverrides` in `frontend/src/utils/applyForm/applyFormUtils.ts` to match date error patterns, reorder ISO date strings into `MM-DD-YYYY`, and output explicit format instructions.
3. Verify the changes by running `npm --prefix frontend test -- applyFormUtils.test.ts` and `npm --prefix frontend run lint`.

**Toolkit**

- Tool: plan-check
- What it produced: An `accept` verdict on `plan.md` and `plan-comment.md` for issue #6192.
- Difficulties: Initial submission rejected for ungrounded diagnosis (reproduction had not yet landed on the live issue thread) and ambiguous date formatting (`MM/DD/YYYY` vs `MM-DD-YYYY`).
- What changed: Posted the reproduction comment to issue #6192, committed strictly to `MM-DD-YYYY` per the acceptance criteria, and defined the exact capture-group regex (`$2-$3-$1`) in `plan.md`.

---

## Testing Strategy

To verify the date error reformatting without relying on unmocked full server forms:

1. Re-ran the reproduction unit assertion in `applyFormUtils.test.ts` passing the raw backend ISO error string (`'275760-01-01' is not a 'date'`). Verified that the output transformed from the raw failing string (`"275760-01-01 is not a date"`) into `"01-01-275760 is not a valid date. Please use MM-DD-YYYY format."`.
2. Verified generic date schema errors (`'must match format "date"'`) reformat to `${formattedTitle} must be a valid date in MM-DD-YYYY format.`.
3. Executed `npm --prefix frontend test -- applyFormUtils.test.ts` passing all 62 tests.
4. Executed `npm --prefix frontend run lint` confirming 0 lint errors. Pre-existing test failures in `withFeatureFlag.test.tsx` and `generalUtils.test.ts` on `main` were verified and documented in `ship-manifest.md`.

**Toolkit**

- Tool: ship-gate & npm jest/eslint
- What it produced: Clean pass across 62 unit tests and 0 lint errors, gating automated PR verification.
- Difficulties: Full test suite failed on `main` due to pre-existing Promise mocks in `withFeatureFlag.test.tsx` and hex escape regex tests in `generalUtils.test.ts`.
- What changed: Scoped active manifest checks to the touched unit suite (`applyFormUtils.test.ts`) and documented full-suite failures under the `Excluded` manifest table with dated reasons.

---

## Implementation Notes

**2026-11-02**

Reproduced the issue locally and posted verbatim Jest output to GitHub issue #6192 (comment 6590808029). Drafted `plan.md` targeting `validationWarningOverrides` in `applyFormUtils.ts`. Evaluated the regex replacement pattern against the issue screenshot to ensure format alignment with `MM-DD-YYYY`.

**2026-11-03**

Implemented date regex pattern matching inside `validationWarningOverrides`. Initial test execution revealed that 6-digit years (such as `275760`) left leading digits intact when matched with `\d{4}`; expanded the capture group to `\d{4,}` to consume extended years cleanly. Resolved TypeScript linting errors regarding unused variables, verified all 62 unit tests passed, and cleared the ship-gate pipeline for PR creation.

**Toolkit**

- Tool: Claude Code, Gemini & git
- What it produced: Clean commit on branch `fix/6192-date-validation-format` updating `applyFormUtils.ts` and `applyFormUtils.test.ts`.
- Difficulties: Regex matched only 4 digits of the 6-digit year in the repro case (`2701-01-5760`), and ESLint flagged an unused title variable during defensive parameter handling.
- What changed: Widened regex year pattern to `\d{4,}` and confined changes strictly to `validationWarningOverrides`.

---

## Pull Request

**PR Link:** [https://github.com/HHS/simpler-grants-gov/pull/13119](https://github.com/HHS/simpler-grants-gov/pull/13119)

**PR Description:** Resolves date validation error messages in application forms displaying raw backend ISO `YYYY-MM-DD` strings rather than matching the user-facing `MM-DD-YYYY` format. Updates `validationWarningOverrides` in `applyFormUtils.ts` to reorder date components into `MM-DD-YYYY` with explicit user guidance, handles generic date format strings, and adds unit tests in `applyFormUtils.test.ts`.

**Maintainer Feedback:**

- 2026-11-03: Opened PR; awaiting initial CI review and maintainer assignment.

**Status:** Awaiting review

- Tool: ship-gate & gh CLI
- What it produced: Pull request opened successfully against `HHS/simpler-grants-gov:main` with compliant description, issue linkage, and AI disclosure.
- Difficulties: PR creation initially rejected due to cross-fork branch target mismatch and template heading omissions.
- What changed: Passed `--head rdv-chio:fix/6192-date-validation-format` and structured `pr_draft.md` to conform to the repo's PR template headings.

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
