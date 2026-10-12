# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

Follow this operating procedure step by step to grade a candidate pull request package.

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides where the plan sits in the order (before the diff? before the
description?) and says why the order matters for the side-by-side
checks that come later. -->

1. **Repo Standards & Policies**: Read `repo_facts` (or live `CONTRIBUTING.md` and PR template). Note required template sections, checklists, and explicit AI disclosure policies.
2. **Plan & Stated Scope**: Read the plan context (`plan_markdown` or `plan.md`), noting the in-scope boundary, out-of-scope declarations, test plan requirements, and any documented deviations.
3. **Unified Diff & Commits**: Read the diff hunks and commit messages. Compare changed files and lines directly against the plan's declared scope and check for debris or churn.
4. **Test Evidence**: Read the test evidence section and transcripts. Compare the test runs against the reproduction commands and expected outcomes from the plan.
5. **PR Title & Description**: Read the candidate PR title and description. Compare its claims against the diff, and check adherence to template sections and policy requirements.

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which file, diff,
or page per your evidence guide) to pull the fact from, and what to
record. The load-bearing gathers this week are pairings: diff files
against plan scope, claimed evidence against the plan's test plan and
the repo's checks, description claims against diff contents. A
complete procedure leaves no check whose evidence an executor would
have to hunt for. -->

- **Plan Fidelity**: Extract changed files from the diff. Check each against the plan's scope and deviation notes. Compare description claims against actual diff changes.
- **Test Evidence**: Locate before and after execution logs. Verify that the reproduction failure command was re-run on the bug path and that repository test checks are visible.
- **Diff Quality**: Inspect diff hunks for debug print statements, commented-out dead code, unused functions, and gratuitous formatting/indentation churn.
- **Standards & Comms**: Check whether PR template sections and required checklists are filled. Verify if repository policy requires AI disclosure and whether disclosure is present.

## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

Execute the checks in this sequence:
1. `plan_fidelity`
2. `decisive_test_evidence`
3. `clean_reviewable_diff`
4. `standards_compliance`

For each check:
- If the pass condition is met, grade `pass` and cite a specific quote or fact.
- If the pass condition is violated, grade `fail` and cite the exact file, hunk, missing evidence, or omitted disclosure.
- If required evidence cannot be verified, grade `fail` (or `unclear` which evaluates to fail).

## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check: the
check whose failure the verdict turned on. When more than one check
failed, your procedure picks which one gets quoted (first failing
required check in rubric order is a fine rule); nothing picks it for
you, so write the rule down. A complete procedure produces the same
verdict from the same grades, every time. -->

1. If all required checks pass, assign `accept`.
2. If one or more required checks fail or are unclear, assign `reject`.
3. In the summary output, quote the evidence from the first failing required check in rubric order.
4. Output the readable summary followed immediately by the final fenced JSON block.