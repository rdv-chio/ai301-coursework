# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

<!-- Where the plan states its scope, boundary, and deviation notes,
and where the diff shows what actually changed. What it means for a
diff to match the plan, for an honest deviation to re-tie a mismatch,
and what silent drift looks like in each direction (more than the
plan, or less with no note). The description's fidelity claims read
against the diff live here too: a description claiming more or less
than the diff delivers is silent drift, not a comms problem. -->

- **Where it lives**:
  - In eval mode: Compare the unified diff (`diff`) and candidate description against the plan scope ("In:", "Out:", "Change:") and deviations in the package bundle.
  - In live mode: Compare `git diff main...HEAD` and `pr_draft.md` against `plan.md` (specifically Scope and `## Deviations`).
- **What good looks like**: Every changed file and hunk falls within the plan's bounded scope. If secondary items were deferred or modified, they are honestly called out in deviations and the description. The description does not claim docs updates, flag changes, or refactors that are absent from the diff.

## Test evidence (harness category: not-tested)

<!-- Where the PR shows its proof: the test-evidence section's
before/after against the plan's test plan and the reproduction's own
steps, and the outcome of the repo's own checks or suite. What
decisive looks like (an observable behavior named, the expected-after
stated, the checks' outcome visible) next to "tests pass". -->

- **Where it lives**:
  - In eval mode: The test evidence block, reproduction before/after logs, and test execution transcripts.
  - In live mode: `test_evidence.md` and the Testing section of `pr_draft.md`.
- **What good looks like**: Shows terminal output with observable before/after verification directly executing the failing repro path (e.g., exit codes, specific error messages resolving, visual states flipping). Running only the passing control path or asserting vague statements like "tests pass" or "tested locally" fails.

## Diff quality (harness category: unreviewable)

<!-- Where the change itself lives: the unified diff and the commit
list. What a reviewable change looks like (the fix visible, nothing
unrelated riding along) and the debris tells: debug leftovers, dead
code, commented-out blocks, formatting churn, drive-by edits. -->

- **Where it lives**:
  - In eval mode: The unified diff hunks and commit message list.
  - In live mode: `git diff main...HEAD` and `git log main..HEAD`.
- **What good looks like**: The diff contains solely the minimal fix. It is free of debug statements (such as `console.log`, `eprintln!`, `print()`), commented-out failed attempts, unused probe functions, and accidental mass reformatting or whitespace churn.

## Standards and comms (harness category: standards-wall)

<!-- Where the repo states its asks (the repo-facts block's PR
template sections, contributing instructions, and stated policy,
including AI-use disclosure) and where the PR honors them: the
stated sections filled with real content, the disclosure present,
explicit maintainer direction in the thread engaged. What compliant
looks like next to boilerplate or a visibly ignored ask. (Whether
the description's claims match the diff is plan fidelity, above.) -->

- **Where it lives**:
  - In eval mode: `repo_facts` (PR template instructions, checklists, contributing policy, AI rules) compared against the PR description.
  - In live mode: Repository `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` compared against `pr_draft.md`.
- **What good looks like**: Preserves and completes the required repository PR template sections and checklists. If the repository policy mandates AI disclosure (e.g., Ghostty or CodePath requirements), an explicit AI disclosure statement must be present in the description.