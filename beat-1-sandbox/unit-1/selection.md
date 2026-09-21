# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72

**Verdict output**

```
Issue #72: verify_password raises UnknownHashError on malformed stored hashes instead of returning False

Check results:

repo_active: pass — Not archived, last push 2026-09-16 (4 days ago)

maintainer_responsive: pass — Opened by Andrew Burke, who authored recent commits

bounded_scope: pass — Clear security bug: UnknownHashError should fail closed (return False), with existing xfail test to remove

issue_unclaimed: pass — No assignees, no open linked PRs, no prior claims

ai_policy_compatible: pass — No explicit AI ban found

newcomer_labeled: pass — Labeled "good first issue", "tier-1", "api"

Verdict: accept

[
{
"item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72",
"checks": [
{"name": "repo_active", "grade": "pass", "evidence": "Not archived; last push 2026-09-16, 4 days ago"},
{"name": "maintainer_responsive", "grade": "pass", "evidence": "Issue opened by Andrew Burke, who authored recent commits"},
{"name": "bounded_scope", "grade": "pass", "evidence": "Clear error-handling bug: UnknownHashError should return False, not raise; test marked xfail with reference"},
{"name": "issue_unclaimed", "grade": "pass", "evidence": "No assignees, no open linked PRs, no prior claims"},
{"name": "ai_policy_compatible", "grade": "pass", "evidence": "No CONTRIBUTING.md, no AI policy files found; silence passes"},
{"name": "newcomer_labeled", "grade": "pass", "evidence": "Labeled: good first issue, bug, api, tier-1"}
],
"verdict": "accept"
}
]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

- agreement: 2/3 scored items
- agreement: 17/20 scored items  (bar: 18/20: below the bar)
- agreement: 0/2 scored items
- agreement: 18/20 scored items  (bar: 18/20: PASS)

**Issue analysis**

- Scored issue: issue-19
- Rubric decision: reject
- Gold label: accept
- Reasoning: The issue was titled "Selecting large subgraphs in proof mode freezes the UI", labeled "Priority: High" and "Type: bug". My earlier bounded_scope check required tasks to be strictly localized and treated performance/UI freezes without explicit short reproduction scripts as complexity risks. The gold label accepts this issue because it was opened directly by a collaborator who already analyzed the root issue and listed two concrete, potential causes in the description, making the fix bounded and actionable for a newcomer.

**Check rationale**

| bounded_scope | Issue body, title, labels, and comment thread | The issue represents an approachable, bounded task (e.g., documentation updates, bug fixes with identified causes, localized feature additions, or test additions). It passes even if brief, checklist-based, missing reproduction steps, or labeled high-priority, as long as it does not explicitly require a multi-subsystem architectural overhaul, an open-ended design debate, or a major cross-framework rewrite. Pure usage questions ("how do I...") fail. | required |

This check targets the failure mode where newcomers choose issues that turn into unbounded architectural redesigns or multi-module refactors. It sets an observable pass condition by accepting bounded bug fixes, documentation, and tasks where maintainers have already diagnosed potential causes or defined a checklist, while explicitly rejecting architectural overhauls, umbrella epics, and support inquiries.

**Trade-offs**

By expanding bounded_scope to accept issues that are brief, checklist-based, or labeled high-priority provided they lack explicit architectural redesign markers, the rubric risks admitting items that seem simple initially but involve subtle state concurrency or cross-module side effects (such as issue-10, where my rubric graded accept while the gold label rejected). I accept this trade-off because overly stringent scope constraints caused false rejections on straightforward documentation tasks (issue-01) and maintainer-diagnosed bugs (issue-19).


---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. The issue's fit to your interests and to the time available.
Issue #72 focuses on Python backend error handling and password verification logic, which matches my background with Python development, error handling, and test-driven fixes. It is a well-isolated bug with a narrow scope, fitting cleanly within the time available for Unit 2 without requiring frontend setup or external services.
2. What the verdict identified correctly, and what you weighed that the rubric could
   not.
The verdict correctly determined that the repository is active, the issue has no active assignees or open linked PRs, and the scope is tightly bounded. Beyond what the rubric evaluates, I weighed the fact that the repository already contains an existing xfail test specifically covering this malformed hash behavior, providing an immediate, objective reproduction and validation check.
3. The anticipated difficulty in claiming it.
The anticipated difficulty in claiming it is low. Under Path Review classroom rules, other comments do not block claiming, and there is no external maintainer bottleneck. The main task will be ensuring the claim comment follows the Unit 2 voice guide and verifying that the fix handles malformed hash strings safely without swallowing unrelated runtime exceptions.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
