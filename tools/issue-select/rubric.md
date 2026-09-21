# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| repo_active | "last 5 default-branch commits" and "archived:" line under Repo facts | The repo is not archived (`archived: false`), and at least one default-branch commit or merged PR occurred within the last 180 days of the capture date. | required |
| maintainer_responsive | "maintainer first-response sample" or maintainer comments under Comments, or issue author | A maintainer (Owner, Member, Collaborator) opened the issue, or commented on the thread, or the repo facts show a maintainer first-response sample within 90 days. Silence on a newly opened or self-contained issue does not fail if repo_active passes. | required |
| bounded_scope | Issue body, title, labels, and comment thread | The issue represents an approachable, bounded task (e.g., documentation updates, bug fixes with identified causes, localized feature additions, or test additions). It passes even if brief, checklist-based, missing reproduction steps, or labeled high-priority, as long as it does not explicitly require a multi-subsystem architectural overhaul, an open-ended design debate, or a major cross-framework rewrite. Pure usage questions ("how do I...") fail. | required |
| issue_unclaimed | "this issue: assignees:" and "linked PRs:" under Repo facts, plus the Comments section | The issue has no assigned users (`assignees: none` or empty), no open linked PRs, and no recent human commenter within 14 days has an active, accepted claim. Closed/unmerged PRs do not fail this check. | required |
| ai_policy_compatible | "contribution policy" and AI policy line under Repo facts, or CONTRIBUTING.md summary | The repository does not explicitly ban AI-generated or AI-assisted contributions (e.g., "we do not accept AI-generated code"). Silence, disclosure requirements, or human-review requirements pass. | required |
| newcomer_labeled | Labels listed under Repo facts or issue header | The issue carries an explicit newcomer-friendly label (such as `good first issue`, `good-first-issue`, `beginner`, `starter`, or `help wanted`). | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

An issue receives a verdict of `accept` if and only if every `required` check passes (`pass`). 
If any `required` check fails (`fail`) or is determined to be `unclear`, the verdict is `reject` with the name of the first failing or unclear check. 
`preferred` checks never alter the accept/reject verdict; they are used solely to order accepted issues.