# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| env_recorded | Repro report text, under environment/setup header or code block | Names the operating system (e.g. Ubuntu 22.04, macOS 14, Windows) and primary runtime or dependency version (e.g., Python 3.11, package version, or git commit SHA). | required |
| steps_reproducible | Repro report reproduction steps / code snippet | Provides the exact CLI commands, code snippet, or test execution steps needed to trigger the behavior end-to-end without requiring a stranger to guess inputs or file paths. | required |
| outcome_documented | Repro report observed vs expected behavior | Displays the verbatim output, stack trace, error log, or symptom observed, and explicitly notes what should have happened instead (or confirms failure under test). | required |
| claim_voice | Claim comment text | Promises an investigation or reproduction attempt (e.g., "Looking into reproducing this...") rather than promising a guaranteed fix, claiming authority, or stating an arbitrary delivery deadline/date. | required |
| policy_compliance | Claim/repro comment text and repo contribution/AI policy | Fully adheres to repository contribution rules (e.g., includes explicit AI disclosure if the repository's policy mandates AI attribution/disclosure). Silence in repo policy passes. | required |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
An evaluation package receives a verdict of `ready` if and only if every `required` check passes (`pass`).
If any `required` check fails (`fail`) or is determined to be `unclear` (`?`), the verdict is `hold` with the name of the failing check.