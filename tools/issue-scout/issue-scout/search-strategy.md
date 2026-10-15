# Search strategy: where your scout looks, and why

<!--
THIS IS THE PART YOU WRITE. The scout in SKILL.md executes whatever
queries and filters you define here. It ships empty on purpose: the
strategy is your judgment about where YOUR first contribution lives,
and a scan without authored queries is just someone else's defaults.

A filled strategy must contain:

1. At least one query in the Queries section. Each query is a real
   `gh search issues` invocation, and each should trace to a sentence
   of your fit profile in scope.md: if you cannot say which sentence
   produced a query, the query is probably not yours. Search breadth
   is the point at this stage: prefer generous limits and let the
   ranking filters do the narrowing.

2. At least one ranking filter in the Ranking filters section. A
   filter is a condition or an ordering over fields the search
   results already carry (last activity date, assignee presence,
   label set, comment count, repo). It must be checkable from the
   result fields alone: if deciding it would need opening the issue
   page, it belongs in your rubric, not here. Ranking is the free
   stage; keep it free. Make at least one filter an ordering, or
   state your tie-break: with drop conditions alone, the scout takes
   the top of whatever order the searches happened to return, and
   the graded candidates stop being traceable to you.

3. The top-N line, kept or deliberately changed.
-->

## Queries

<!-- Real gh search invocations, one per line, each with a comment
naming the fit-profile sentence it came from. Example SHAPE (write
your own; these values belong to nobody):

gh search issues --label "good first issue" --language python --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url
-->

# From: "I prefer well-isolated bug fixes, CLI/utility issues... in Python"
gh search issues --label "good first issue" --language python --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url

# From: "clear logic or typing errors where expected input/output behaviors are specified"
gh search issues --label "good first issue" --language python --state open "type" --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url

# From: "proficient in Python, data processing workflows... and core software testing"
gh search issues --label "good first issue" --language python --state open "test" --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url


## Ranking filters

<!-- Conditions and orderings over the free result fields, applied in
the order written. State each so a stranger could apply it and get
your ordering. Example SHAPES (write your own):

- drop any result with an assignee
- drop any result not updated in the last 60 days
- prefer fewer comments over more
-->

1. Drop any result where `assignees` is not empty (filter out claimed issues).
2. Drop any result where `updatedAt` is older than 60 days (filter out stale or abandoned threads).
3. Drop any result whose title mentions frontend terms: "css", "html", "react", "vue", "ui", "frontend", or "webpack".
4. Prefer fewest comments first (`commentsCount` ascending; issues with 0 or 1 comment are less likely to have hidden active claim disputes).
5. Tie-break: Prefer most recently updated (`updatedAt` descending).

## How many to grade

Grade the top **5** ranked results per scan.

<!-- Five is the default because grading is the only stage that costs
money: about $0.20 per candidate, so a scan is about $1, and you can
iterate the strategy several times for less than one full regression
run. You need one good issue, not ten. Raising this number is
allowed, but it is a decision you state here in a sentence, with the
cost you are accepting; it is not a number you bump because a scan
disappointed you once. -->

Grade the top **5** ranked results per scan.
