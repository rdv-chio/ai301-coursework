# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/159

**Branch**

fix/72-verify-password-hash

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

20/20

**Package analysis**

pkg-20: The gold label was reject, and our rubric decided reject (failed: standards_compliance). In pkg-20, the candidate authored a clean, bounded fix implementing the maintainer diagnosis with decisive mode-2031 evidence. However, Ghostty's stated repository contribution policy explicitly mandates disclosing all AI usage. Because the candidate PR description omitted the required AI-use disclosure, our rubric evaluated standards_compliance as fail, successfully catching the critical standards-wall category canary.

**Check rationale**

"| plan_fidelity | Unified diff and description claims read against plan scope & deviations | The diff implements the planned changes without introducing unannounced scope creep, unrelated refactors, or new configuration flags not in the plan. Any omitted planned item or scope modification must be explicitly documented in plan deviations and stated in the description. The description must not claim changes the diff does not contain. | required |"

This check was designed after evaluating calib-03, where the candidate presented an apparently pristine checklist and claimed fidelity to the plan, yet silently modified an unrelated file (print/property.js) in the diff. Demanding that the diff be directly reconciled against the plan's declared scope boundaries and deviations ensures that unannounced scope drift is caught even when masked by a well-formatted description.

**Trade-offs**

By making standards_compliance a required check that strictly enforces repository templates, checklists, and explicit AI disclosure requirements, we ensured our rubric caught all required standards canaries (such as pkg-01 and pkg-20). The trade-off is that any candidate PR in a repository with mandatory disclosure or template policies is rejected if those requirements are bypassed, even if the underlying code change and test evidence are flawless. This maintains parity with real maintainer review gates and produced a 20/20 agreement score across all evaluation categories.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
