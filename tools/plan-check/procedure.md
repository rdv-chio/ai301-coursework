# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides the order (issue first? repro evidence first?) and says why
the order matters for the checks that come later. -->

1. **Repo facts & Thread Context**: In eval mode, read `repo_facts` and `thread_highlights`. In live mode, read `CONTRIBUTING.md` and existing issue thread comments. Note any AI disclosure requirements, contribution rules, and maintainer guidance.
2. **Issue Description & Repro Evidence**: Read the issue description, followed by the reproduction report and control runs. Note the exact failure mechanism, reproduction command, and control results that pin down the bug.
3. **Plan Package**: Read `plan_markdown` (or `plan.md`) completely: diagnosis, scope boundaries, approach, named files, and test plan.
4. **Draft Comment**: Read `plan_comment_markdown` (or draft comment) to evaluate communication tone, thread engagement, and required disclosures.

*Why this order matters*: You cannot evaluate whether a diagnosis is grounded or a scope is bounded without knowing what the reproduction actually proved and what the maintainers directed.

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which page or
thread location per your evidence guide) to pull the fact from, and
what to record. A complete procedure leaves no check whose evidence an
executor would have to hunt for. -->

Gather evidence for each rubric check using the mappings in `references/evidence-guide.md`:

- **Diagnosis**: Extract the stated cause from the plan. Extract the failure symptom and control outcomes from the repro evidence. Verify whether the repro or control data contradicts the stated cause.
- **Scope**: Extract the in-scope actions, out-of-scope boundaries, and any included refactors/migrations. Verify if the change is confined to the minimal fix.
- **Executability**: Extract target files, functions, or layers. Determine if an approach is explicitly chosen or if core decisions are left open-ended.
- **Test plan**: Extract the test commands and the stated observable success condition. Verify whether an observable outcome is named.
- **Thread direction**: Extract maintainer directives from thread highlights. Check if the candidate plan/comment acknowledges or contradicts them.
- **Policy compliance**: Extract AI disclosure or contribution constraints from repo facts. Check if the draft comment complies.

## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

Execute each check defined in `rubric.md` in the order listed:
1. `diagnosis_grounded`
2. `bounded_scope`
3. `executability`
4. `decisive_test_plan`
5. `thread_direction`
6. `policy_compliance`

For each check:
- If evidence satisfies the pass condition, assign `pass` and record a one-line quotation or fact.
- If evidence violates the condition, assign `fail` and record the specific contradiction or missing detail.
- If required evidence is missing or ambiguous, assign `fail` (or `unclear` which evaluates as fail under the verdict rule).

## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check. A
complete procedure produces the same verdict from the same grades,
every time. -->

1. If all required checks are `pass`, the final verdict is `accept`.
2. If any required check is `fail` or `unclear`, the final verdict is `reject`.
3. Preferred checks do not alter the final verdict.
4. Emit the per-check summary, followed by the final fenced JSON block adhering strictly to the schema in `SKILL.md`.