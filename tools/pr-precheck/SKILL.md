---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->

## The question

<!-- State, in your words, the one question this tool answers and what
a PR package is: what artifacts it contains and what they are read
against. CONTRACT.md fixes the question; your frame has to say it so
the tool cannot wander into grading something else. -->

This tool answers exactly one question about a candidate PR package: is this pull request ready to submit? A PR package consists of the candidate pull request (title, description, commits, diff, test evidence), read against the plan it claims to implement (including any stated deviations) and the issue that plan belongs to.

## Inputs and modes

<!-- Define both modes. Live mode: name every input (your plan.md with
its deviation notes, your branch's diff, your draft PR title and
description, your test evidence, your issue), where each comes from,
and what a house-chain student reads instead. The branch's diff is
everything the branch changes relative to the repo's default branch:
`git diff main...HEAD` (three dots), run from the working copy, is
the command that produces it; name the source that concretely. Eval
mode: state that the bundle is the whole world, nothing is fetched,
and every check runs with the full verdict rule. -->

- **Live mode**: Grades the student's own pull request before opening it. The tool reads:
  1. `plan.md` in `beat-1-sandbox/unit-3/` (including any documented deviations).
  2. The branch diff generated from `git diff main...HEAD`.
  3. The draft PR title and description in `pr_draft.md`.
  4. The test evidence in `test_evidence.md`.
  5. The upstream issue context, thread comments, repository PR template, and `CONTRIBUTING.md`.
  For house-chain students, the tool reads the house plan and repro pack instead.
- **Eval mode**: The eval package bundle is the entire world. All evidence is extracted strictly from the bundle text (issue, repo facts, plan context, diff, commits, description, test evidence). Nothing is fetched from the network, and the full rubric is evaluated.

## The scope seam (live mode only)

<!-- Tell the tool when to read scope.md, what to do with the rules it
finds there, what to refuse, and what to do when the scope's repo line
is an unfilled placeholder. State that eval mode ignores scope.md
entirely. CONTRACT.md names the required behavior; your frame has to
instruct it. -->

In live mode, read `scope.md` before performing any checks. Verify that the target repository matches the scoped repository. If the `Repo:` line in `scope.md` contains an unfilled placeholder, stop immediately and instruct the student to populate the repository name. If the issue or PR targets an out-of-scope repository, refuse to grade. In eval mode, ignore `scope.md` completely.

## The voice seam (live mode only)

<!-- Tell the tool when to read voice-guide.md, which outgoing text it
gates (the PR title and description), how to report a broken rule, and
why it never changes the verdict on its own. State that eval mode
ignores it entirely. -->

In live mode, read `voice-guide.md` and verify the candidate PR title and description against the student's voice rules. Report any violations in the readable summary. The voice guide does not alter the rubric verdict on its own. In eval mode, ignore `voice-guide.md` completely.

## Component reads

<!-- Tell the tool how the pieces connect: rubric.md defines the
checks and the verdict rule, references/evidence-guide.md maps where
each evidence family lives, procedure.md is executed as written. Say
what the tool does when the procedure is silent on a step (report the
gap, never improvise around it) and what it does when rubric.md or
procedure.md has no content (the refusal rule, stated as an
instruction). -->

1. `rubric.md`: Defines the checks table and the verdict rule.
2. `references/evidence-guide.md`: Maps the location and acceptance criteria for each evidence family.
3. `procedure.md`: Specifies the operational workflow (read order, evidence gathering, check execution, verdict assembly). Follow `procedure.md` exactly as written. If the procedure is silent on a scenario, report the procedural gap in the summary rather than inventing steps. If `rubric.md` or `procedure.md` is empty or unauthored, refuse to grade.

## Verdict and output

<!-- State the binary verdict space (accept means submit, reject means
hold) and instruct the tool to end its reply with the fenced JSON
block below, valid and last, with nothing after it. The schema is
CONTRACT.md's, verbatim; do not edit it. -->

The verdict space is strictly binary: `accept` (ready to submit) or `reject` (hold).

End the output with the required fenced JSON block, valid and last:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

<!-- Write the standing rules the tool grades under: evidence first,
grade the thing not the polish, the rubric decides, the procedure
decides how, and how unclear grades are treated when the rubric's
verdict rule is silent. CONTRACT.md states each as a guarantee; your
frame has to make them instructions. -->

* __Evidence first:__ Every check grade must cite a concrete fact, quote, or diff excerpt that decided it.

* __Grade the thing, not the polish:__ Evaluate code changes, test execution, and factual adherence rather than persuasive writing or cosmetic formatting.

* __The rubric decides:__ Do not override rubric pass conditions based on subjective impressions.

* __The procedure decides how:__ Execute the procedural steps as authored.

* __Unclear defaults to fail:__ If required evidence cannot be verified from the package, the check evaluates to fail.
