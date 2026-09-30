# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->


## Diagnosis and grounding

<!-- Where the plan states its cause, and where the repro evidence
pins down the behavior that cause must explain. What it means for a
diagnosis to follow from the evidence rather than contradict or
ignore it. -->

- **Where it lives**:
  - In eval mode: Compare the "Cause" / "Diagnosis" in `plan_markdown` against the observations and control runs in `repro_evidence_markdown` and `issue`.
  - In live mode: Compare the student's `plan.md` diagnosis against the posted reproduction comment on GitHub and any control steps.
- **What good looks like**: The explanation accounts for why the failure happened under the repro conditions and does not contradict any control test (for example, blaming a parser when a control run without flags succeeded).

## Scope

<!-- Where the plan bounds itself: the in-scope statement, the
not-in-scope line, the files or areas named. What one bounded change
looks like next to a drive-by rewrite. -->

- **Where it lives**:
  - In eval mode: The "In:" and "Out:" / "Change:" statements in `plan_markdown`.
  - In live mode: The scope pair (in-scope and not-in-scope) and files list in `plan.md`.
- **What good looks like**: The fix targets the isolated bug and specifically bounds out adjacent cleanup, library migrations, new options, or broad architectural overhauls. Deferrals of complex secondary refactors are stated clearly.

## Executability

<!-- Where the plan says what will actually be done: files or areas,
approach, order of work. What it means for a stranger to be able to
start executing without asking the author anything. -->

- **Where it lives**:
  - In eval mode: The approach description and named files/functions in `plan_markdown`.
  - In live mode: The proposed implementation steps and target code paths in `plan.md`.
- **What good looks like**: Concrete target files, methods, or controllers are named with a clear sequence of work. A developer unfamiliar with the author can sit down and immediately write the code without guessing architecture.

## Test plan

<!-- Where the plan says how success will be observed, and how that
maps onto the repro evidence's steps and artifacts. What a decisive
test plan names that a vague one does not. -->

- **Where it lives**:
  - In eval mode: The "Test:" section in `plan_markdown`.
  - In live mode: The test plan in `plan.md`.
- **What good looks like**: Directly references the reproduction steps or automated test command, stating the observable delta (e.g., "re-run step 3: commit turns green", "test passes with exit code 0", "returns False instead of raising UnknownHashError"). Generic "run full test suite" without specific assertions fails.

## Honesty

<!-- Where claims meet uncertainty: risks, unknowns, and deviations.
How to tell stated unknowns from false confidence, and where an
honest mid-build deviation gets recorded. -->

- **Where it lives**:
  - In eval mode: Risks, unknowns, and explicit deferrals in `plan_markdown`.
  - In live mode: Risks, unknowns, and mid-build deviation notes in `plan.md`.
- **What good looks like**: Genuine uncertainties (platform edge cases, secondary platforms) are called out as checked unknowns or deferred scope, rather than masked by false certainty.

## Comms

<!-- Where the words meet the thread and the repo: the plan comment
read against the issue's maintainer signals (thread highlights, or
the live thread) and against the repo-facts block's stated templates,
contributing asks, and contribution policy (including AI-use
disclosure requirements). What thread-aware looks like next to
boilerplate. -->

- **Where it lives**:
  - In eval mode: `plan_comment_markdown` read against `repo_facts` (specifically `contribution policy`) and `thread_highlights`.
  - In live mode: The draft plan comment in `comment.md` read against GitHub issue comments, PR discussions, and repo `CONTRIBUTING.md`.
- **What good looks like**: Follows maintainer direction from the thread, respects review bandwidth, avoids unkept delivery promises, and includes explicit AI-use disclosure whenever the repository's contributing policy requests or mandates it.