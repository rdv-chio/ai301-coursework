# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

<!-- Where the environment record lives, and what a sufficient one
looks like against the issue's stated target. -->
- **Where it lives:**
  - In eval mode: Under the `## Candidate repro report` section, typically under an `Environment` header, bullet list, or introductory paragraph.
  - In live mode: Inside the draft repro comment under an environment/setup section, or the repo's README/setup docs.
- **What good looks like:**
  Explicitly identifies the operating system (e.g., Ubuntu 22.04, macOS 14, Windows 11) and the runtime, library, or tool version (e.g., Python 3.11, yq 4.53.3, or git commit SHA). If testing on a version differing from the original issue report, the difference is acknowledged.

## Steps

<!-- Where the reproduction steps live, and what makes them followable
by a stranger, starting state to trigger. -->
- **Where it lives:**
  - In eval mode: Under `Preparation`, `Execution`, `Steps to Reproduce`, or fenced shell/code blocks in the repro report.
  - In live mode: Inside the draft repro comment's commands or setup block.
- **What good looks like:**
  Contains the verbatim CLI commands, test runner commands (e.g., `pytest -k ...`), or input scripts necessary for an outside engineer to execute the reproduction end-to-end without guessing missing arguments, flags, or prerequisites.

## Behavior shown

<!-- Where the artifacts live (output excerpts, logs, screenshots),
and what it means for an artifact to show the issue's behavior rather
than an adjacent one. -->
- **Where it lives:**
  - In eval mode: Under `Actual behavior`, `Output`, `Analysis`, or fenced log/traceback blocks in the candidate repro report, compared directly against `## Issue`.
  - In live mode: Terminal output excerpts, stack traces, or test failure logs pasted in the comment.
- **What good looks like:**
  The output excerpt or exception matches the symptom described in the original issue (e.g., reproducing the exact reported panic or error message). If the output reveals a user-introduced syntax typo or an unrelated tool failure, this fails.

## Honesty

<!-- Where claims and their backing meet: how to tell a report that
says exactly what happened (including an honest cannot-reproduce) from
one that claims more than its evidence shows. -->
- **Where it lives:**
  - In eval mode: The conclusion/analysis statements in `## Candidate repro report` evaluated against the evidence in the same report.
  - In live mode: The summary sentences in the repro comment.
- **What good looks like:**
  The candidate's claims accurately reflect what the run produced. If the bug reproduced, it states that with proof. If the bug could not be reproduced under stated conditions, an honest "cannot reproduce" statement backed by output passes. A report claiming successful reproduction when the log shows an unrelated error fails.

## Comms

<!-- Where the words meet the repo: the claim comment against the
issue, the comments against the repo's stated templates and
contribution policy (including AI-use disclosure requirements), and
what specific-and-honest looks like next to boilerplate. -->
- **Where it lives:**
  - In eval mode: `## Candidate claim comment` and `## Repo facts` (specifically `contribution policy`).
  - In live mode: Draft claim text read against the repo's `CONTRIBUTING.md` or AI policy file.
- **What good looks like:**
  The claim comment promises an investigation rather than guaranteeing a solution, claiming authority, or stating an arbitrary delivery date. If the repo policy mandates AI disclosure, the candidate comments explicitly include that disclosure. Silence in repo policy passes.
