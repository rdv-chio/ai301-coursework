# Scan notes: friction log

Each entry is one thing the scan made harder than it should be, with the
patch it suggests. Newest first.

## 2026-10-15: first live scan (3 queries, top 5 graded)

- **Fewest-comments ordering is a no-op on this field.** Every one of the
  first 12 survivors had `commentsCount: 0`, so the tie-break
  (`updatedAt` descending) alone decided the top 5. Patch idea: state the
  tie-break as the real ordering, or add a filter that uses the
  repo-maturity fields (stars, `pushedAt` vs repo creation date).
- **The top 5 were all brand-new repos.** Four of five had been created
  within hours of the scan (first commit the same day, 0-2 stars). The
  60-day `updatedAt` drop and `repo_active` (180 days) both pass these,
  so the rubric accepts them. Patch idea: a preferred check on repo age or
  stars, or a ranking filter that prefers repos with more than one commit.
- **`--json authorAssociation` is not a valid `gh issue view` field.**
  The evidence guide points at `author_association`, which only the REST
  API returns. Patch idea: fetch through `gh api repos/OWNER/REPO/issues/N`
  and name that call in `evidence-guide.md`.
- **Linked-PR search by bare number matches noise.** `gh pr list --search
  "9 in:body"` returned an unrelated open PR (its body mentions "eslint 9").
  Patch idea: search for `#9` or `issues/9` and confirm against the issue
  timeline before grading `issue_unclaimed`.
- **Policy file lookup needs distinct names.** Saving `CONTRIBUTING.md`
  and `.github/CONTRIBUTING.md` under the same basename silently deleted
  the root file on a 404. Not a skill bug, but worth a note in the
  evidence guide if the grader fetches policy files by script.
- **Recording-only issues pass `bounded_scope`.** `kiln#38` (record a demo)
  is not a code change. The rubric's "documentation updates" wording
  accepts it. Fit ranks it last, which is the intended split between the
  rubric (accept) and fit (order).
