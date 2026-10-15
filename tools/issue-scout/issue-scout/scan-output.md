# Scan Output: 2026-10-15

Scan result: 5 of 5 graded candidates accepted

Funnel: 299 unique results, 255 after drops, top 5 taken.

Ranked read-out:
1. kaifcodec/user-scanner#708: register talkme in LOUD_MODULES (Python CLI bug fix)
2. dassjaylabs/filmwright#9: _stem() returns film for non-Latin titles (Python CLI bug fix)
3. deanx/locadora-de-relogios#1: add a stats menu option (New feature, zero stars)
4. antoine-tena/kiln#37: translate the command reference (Docs task)
5. antoine-tena/kiln#38: record a README demo (Docs/demo task)

```json
[
  {
    "item": "[https://github.com/kaifcodec/user-scanner/issues/708](https://github.com/kaifcodec/user-scanner/issues/708)",
    "checks": [
      {"name": "repo_active", "grade": "pass", "evidence": "archived: false; pushedAt 2026-10-15; latest release v1.5.2 on 2026-09-17; default-branch commits 2026-10-15 and 2026-10-12"},
      {"name": "maintainer_responsive", "grade": "pass", "evidence": "opened by kaifcodec, author_association OWNER"},
      {"name": "bounded_scope", "grade": "pass", "evidence": "adds one string to LOUD_MODULES[\"email\"], a docstring update and existing tests; no design debate"},
      {"name": "issue_unclaimed", "grade": "pass", "evidence": "assignees: []; linked PRs: none; comments: 0"},
      {"name": "ai_policy_compatible", "grade": "pass", "evidence": "CONTRIBUTING.md: \"If you contribute with an AI coding agent, the ponytail skill pairs well\"; AGENTS.md is an AI contributor guide"},
      {"name": "newcomer_labeled", "grade": "pass", "evidence": "labels: good first issue, bug fix"}
    ],
    "verdict": "accept"
  },
  {
    "item": "[https://github.com/dassjaylabs/filmwright/issues/9](https://github.com/dassjaylabs/filmwright/issues/9)",
    "checks": [
      {"name": "repo_active", "grade": "pass", "evidence": "archived: false; pushedAt 2026-10-15; default-branch commits 2026-10-15"},
      {"name": "maintainer_responsive", "grade": "pass", "evidence": "opened by dassjaylabs, author_association OWNER"},
      {"name": "bounded_scope", "grade": "pass", "evidence": "\"Small and self-contained: a good first issue\"; fix in _stem() in src/filmwright/build.py with a test location named"},
      {"name": "issue_unclaimed", "grade": "pass", "evidence": "assignees: []; timeline shows only labeled/milestoned events; PR #1 matched only on the string \"eslint 9\", not linked; comments: 0"},
      {"name": "ai_policy_compatible", "grade": "pass", "evidence": "CONTRIBUTING.md states no AI restriction (only a CLA mention); no AI policy file"},
      {"name": "newcomer_labeled", "grade": "pass", "evidence": "labels: good first issue, type: bug"}
    ],
    "verdict": "accept"
  },
  {
    "item": "[https://github.com/deanx/locadora-de-relogios/issues/1](https://github.com/deanx/locadora-de-relogios/issues/1)",
    "checks": [
      {"name": "repo_active", "grade": "pass", "evidence": "archived: false; pushedAt 2026-10-15T15:07:29Z; initial commit 2026-10-15 (repo created the same day)"},
      {"name": "maintainer_responsive", "grade": "pass", "evidence": "opened by deanx, author_association OWNER"},
      {"name": "bounded_scope", "grade": "pass", "evidence": "adds menu option [5] Estatisticas over existing Relogio and Estoque classes; \"Não é urgente — é uma boa primeira contribuição\""},
      {"name": "issue_unclaimed", "grade": "pass", "evidence": "assignees: []; linked PRs: []; comments: 0"},
      {"name": "ai_policy_compatible", "grade": "pass", "evidence": "no CONTRIBUTING.md, no AI policy files; silence passes"},
      {"name": "newcomer_labeled", "grade": "pass", "evidence": "labels: enhancement, good first issue"}
    ],
    "verdict": "accept"
  },
  {
    "item": "[https://github.com/antoine-tena/kiln/issues/37](https://github.com/antoine-tena/kiln/issues/37)",
    "checks": [
      {"name": "repo_active", "grade": "pass", "evidence": "archived: false; pushedAt 2026-10-15T14:54:13Z; default-branch commits 2026-10-15"},
      {"name": "maintainer_responsive", "grade": "pass", "evidence": "opened by antoine-tena, author_association OWNER"},
      {"name": "bounded_scope", "grade": "pass", "evidence": "\"translate one or more sections into docs/reference.md... Partial PRs (one section at a time) are welcome\""},
      {"name": "issue_unclaimed", "grade": "pass", "evidence": "assignees: []; linked PRs: []; comments: 0"},
      {"name": "ai_policy_compatible", "grade": "pass", "evidence": "CONTRIBUTING.md has no AI ban; asks for PRs against develop; no AI policy file"},
      {"name": "newcomer_labeled", "grade": "pass", "evidence": "labels: documentation, good first issue"}
    ],
    "verdict": "accept"
  },
  {
    "item": "[https://github.com/antoine-tena/kiln/issues/38](https://github.com/antoine-tena/kiln/issues/38)",
    "checks": [
      {"name": "repo_active", "grade": "pass", "evidence": "archived: false; pushedAt 2026-10-15T14:54:13Z; default-branch commits 2026-10-15"},
      {"name": "maintainer_responsive", "grade": "pass", "evidence": "opened by antoine-tena, author_association OWNER"},
      {"name": "bounded_scope", "grade": "pass", "evidence": "record a 20–30 s VHS or asciinema demo under 2 MB and add it to README.md; documentation task, not code"},
      {"name": "issue_unclaimed", "grade": "pass", "evidence": "assignees: []; linked PRs: []; comments: 0"},
      {"name": "ai_policy_compatible", "grade": "pass", "evidence": "CONTRIBUTING.md has no AI ban; \"Bug reports, ideas, docs fixes and code are all welcome\""},
      {"name": "newcomer_labeled", "grade": "pass", "evidence": "labels: documentation, good first issue"}
    ],
    "verdict": "accept"
  }
]
```
