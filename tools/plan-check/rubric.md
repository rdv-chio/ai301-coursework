# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis_grounded | Plan's stated cause vs reproduction evidence & control runs | The stated root cause directly accounts for and is consistent with the reproduction evidence, control runs, and observed symptoms. The diagnosis must not contradict control data or blame a component already shown to work. | required |
| bounded_scope | Plan's in-scope and not-in-scope declarations vs issue defect | The plan restricts itself to one bounded fix addressing the reported defect. It does not introduce unsolicited drive-by refactoring, framework migrations, architectural redesigns, or unrelated feature additions. (Explicitly deferring secondary work passes). | required |
| executability | Plan's approach, target files, and order of work | Specific files, components, or entry points are identified, and the technical approach is concrete enough that a stranger could begin execution without making foundational architectural choices. (Deferring core technical decisions to build time fails). | required |
| decisive_test_plan | Plan's test plan vs reproduction steps | The test plan re-runs the reproduction steps or targeted test suite and explicitly specifies the observable post-fix outcome (e.g., expected return value, error disappearance, visual flip, or exit code). Merely stating "run tests" or subjective criteria like "should feel fast" fails. | required |
| thread_direction | Plan comment & approach vs maintainer thread highlights | The proposed plan and comment engage with, and do not contradict or ignore, explicit maintainer guidance, isolated locations, or consensus established in the issue thread. | required |
| policy_compliance | Plan comment vs repo_facts contribution policy (including AI disclosure) | The draft comment complies with all contribution rules in repo_facts (e.g., required AI-use disclosure statements, PR review policies, or comment conventions). If the repo requires AI disclosure, disclosure must be present. | required |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->

Accept if every required check passes. Reject if any required check fails or is unclear. Preferred checks never change the verdict on their own.