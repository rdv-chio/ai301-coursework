# Toolkit Portfolio

**Student:** Rocio Dominguez Vidana

## Featured Tools

I chose `issue-scout`, `ship-gate`, and `pr-monitor` because they represent the core lifecycle transitions of my open-source contribution: discovering bounded work without false claims, enforcing repository-grade verification before opening code to public maintainers, and automating post-ship check-ins grounded in honest quiet thresholds. Each tool moved visibly from a generic initial script into a calibrated gate shaped directly by the frictions encountered in `HHS/simpler-grants-gov`.

**issue-scout**

**What it is, and the judgment hole I wrote:** An automated issue discovery and filtering tool that parses GitHub issue queues to identify viable contribution candidates matching my engineering profile. The judgment hole I wrote is `rubric.md` and `search-strategy.md`, which define the ranking weights, eliminate active or ambiguous human claims, and establish scope feasibility thresholds to reject toy repositories and unbounded design tasks.

**How it evolved, dated:**

- 2026-10-14: Initial ranking by fewest comments proved to be a no-op because the top 12 scan results all had zero comments, leaving `updatedAt` as an accidental tiebreaker.
- 2026-10-15: The baseline `repo_active` rule accepted brand-new, zero-star repos created on the scan date. Added explicit non-docs bug label filtering and a minimum star/activity heuristic.
- 2026-10-21: The literal `issue_unclaimed` check passed candidate `dna#127` despite an unanswered human claim in the thread. Added the `scope-feasible` patch to evaluate thread discussion and distinguish between strictly bounded tasks and contested or open-ended design inquiries.

**Evidence:**

Grading output evaluating `HHS/simpler-grants-gov#6192`:

```text
issue-scout: evaluating HHS/simpler-grants-gov#6192
  check repo_active: PASS (active within 7 days, 4.2k stars)
  check scope_feasible: PASS (bounded formatting fix, clear acceptance criteria)
  check issue_unclaimed: PASS (0 assignees, 0 competing PRs, 0 active claims)
verdict: accept (difficulty: moderate)
```

**ship-gate**

**What it is, and the judgment hole I wrote:** A pre-ship gating harness that executes local verification commands and enforces compliance against repository-specific contribution rubrics prior to PR creation. The judgment hole I wrote is `ship-manifest.md` (defining active required commands versus justified exclusions) and `pr-precheck/rubric.md` (evaluating plan fidelity, decisive test evidence, clean reviewable diffs, and template standards).

**How it evolved, dated:**

- 2026-11-02: Initial full suite check (`npm --prefix frontend test`) failed due to pre-existing failures on `main` in `withFeatureFlag.test.tsx` (Promise mock mismatch) and `generalUtils.test.ts` (hex escape string differences).
- 2026-11-03: Refactored `ship-manifest.md` to split checks into active unit assertions (`applyFormUtils.test.ts`) and a dated exclusions table recording the pre-existing upstream test failures.
- 2026-11-03: `pr-precheck` rejected PR draft on `plan_fidelity` and `standards_compliance`. Confined changes strictly to `validationWarningOverrides`, recorded the `\d{4,}` regex deviation in `plan.md`, and updated `pr_draft.md` to include required upstream headers (`## Changes proposed, ## Context for reviewers, ## Validation steps`) and the `[Issue 6192]` title prefix.

**Evidence:**

Ship-gate live-mode output on branch `fix/6192-date-validation-format`:

```Plaintext
ship-gate: HHS/simpler-grants-gov @ fix/6192-date-validation-format against main
ship-gate: repo check 1/2 Frontend unit tests: ok (68s)
ship-gate: repo check 2/2 Frontend lint: ok (193s)
ship-gate: running your pr-precheck in live mode...
  plan_fidelity [pass]: Diff touches only applyFormUtils.ts and its test; \d{4,} deviation documented
  decisive_test_evidence [pass]: Repro received "275760-01-01 is not a date"; after: 62 passed, lint 0 errors
  clean_reviewable_diff [pass]: 30 insertions, 0 deletions across 2 files; no churn or debug code
  standards_compliance [pass]: Template sections filled, Fixes #6192 present, AI disclosure included
verdict: accept
```

**pr-monitor**

**What it is, and the judgment hole I wrote:** A post-ship command-line inspection tool that checks open pull requests against GitHub's API and recommends grounded next moves without hallucinating or making premature contact. The judgment hole I wrote is `playbook.md`, which codifies non-defensive response rules, exact quiet thresholds, single-touch nudge rules, and explicit walk-away criteria.

**How it evolved, dated:**

- 2026-11-03: Configured `playbook.md` with a strict 7-day initial quiet threshold to prevent premature pings during standard upstream batching cycles.

- 2026-11-03: Ran live monitor against freshly opened PR #13119. The runner successfully evaluated the thread state (`under a day ago`), suppressed model-drafted follow-up comments, and issued an explicit wait note citing the playbook rule.

**Evidence:**

Drafted move execution from `drafted-move.md`:

```Plaintext
MOVE: Wait note (no nudge). The playbook's threshold isn't met.

Wait note: Don't nudge yet. Your playbook says "First response threshold: 7 calendar days of quiet after PR opening is normal rhythm for HHS/simpler-grants-gov before considering a nudge," and "Default to silence if fewer days have elapsed; wait out maintainer batching cycles." PR #13119 was opened 2026-11-03 (under a day ago), and the thread has 0 comments or reviews. The 7-day threshold would be met on 2026-11-10.
```

## Inventory

- issue-select: Initial issue filter from Unit 1; superseded by `issue-scout` after adding scope feasibility and claim checking.

- repro-check: Automated unit reproduction validator that verifies whether a test suite genuinely exercises the failing edge case before planning.

- plan-check: Solution design reviewer that checks root-cause diagnosis, explicit format targeting, and bounded diff scope before code is written.

- voice-guard: PreToolUse hook that inspects outgoing comments for technical grounding, timeframes, and tone while preventing accidental unreviewed pushes.

## The Arc

**Beat 1**
Date: 2026-10-15
Friction: Generic repository metrics in `issue-scout` allowed empty and toy repos to pass while ranking zero-comment issues randomly by date.
Change: Introduced non-docs bug filtering, minimum repository activity thresholds, and the `scope-feasible` rubric rule.
Result: Correctly surfaced and validated `HHS/simpler-grants-gov#6192`, successfully filtering out contested and frontend-heavy candidates.

**Beat 2**
Date: 2026-10-22
Friction: Early reproduction attempts relied on mounting full form components with mock state, which masked the real date parsing failure and produced false negatives.
Change: Extracted the reproduction into an isolated unit test in `repro.test.ts` passing verbatim ISO error strings directly to `formatValidationWarning`.
Result: Yielded unambiguous reproduction evidence (`"275760-01-01 is not a date"` vs expected `MM-DD-YYYY`) that was accepted by `plan-check` and posted to issue #6192.

**Beat 3**
Date: 2026-11-02
Friction: Outgoing PR gating failed because running the entire test suite on `main` encountered pre-existing failures in `withFeatureFlag.test.tsx` and `generalUtils.test.ts`.
Change: Updated `ship-manifest.md` to isolate active verification to the affected utility suite (`applyFormUtils.test.ts`) while documenting pre-existing main failures with dates and justifications in the exclusion table.
Result: Allowed local verification to complete cleanly (62 passed tests, 0 lint errors) and passed ship-gate verification without masking genuine regressions.

**Beat 4**
Date: 2026-11-03
Friction: Automated PR submission failed due to missing upstream template sections and cross-fork origin mismatches.
Change: Aligned `pr_draft.md` with upstream template headers, added `[Issue 6192]` title prefix, and explicitly passed `--head rdv-chio:...` to GitHub CLI.
Result: Pull request #13119 was created cleanly on `HHS/simpler-grants-gov:main` with full automated check approval.

## Limitations and What's Next

The toolkit currently provides strong pre-flight validation and post-ship monitoring, but still relies on manual intervention to bridge mock environments with upstream CI. In particular, the toolchain cannot independently inspect remote GitHub Actions logs when local test configurations diverge from containerized CI environments. In future iterations, I would integrate automated remote CI log extraction into `pr-monitor` and develop an AST-based parser in `plan-check` to flag scope creep across helper utilities automatically before linting runs.

**Tool limits.**

issue-scout: Does not parse conversation sentiment in deeply nested issue threads, occasionally requiring manual review to detect soft claim consensus.
ship-gate: Cannot dynamically differentiate between flaking network tests and genuine regressions on the target branch without manual exclusion entries.
pr-monitor: Relies on local execution rather than webhooks, requiring manual command invocation to refresh PR lifecycle status.
