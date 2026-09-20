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
| Maintainer is active | repo-facts block and issue thread: the date of the last maintainer commit to the default branch, AND the date of the last maintainer comment on the issue thread — either one on its own counts as a signal of life | Last maintainer comment on the issue OR last default-branch commit is within 30 days of the reference date (the bundle's capture date in eval mode; today in live mode) | required |
| Repo is in active use | repo-facts block: dates of the last 5 commits to the default branch, and open/closed issue counts if available | At least 2 of the last 5 default-branch commits fall within 90 days of the reference date (the bundle's capture date in eval mode; today in live mode) | required |
| Nobody else is already on it | issue body and full comment thread: assignee field, and any comment claiming the issue ("I'll take this", "working on it", linked open PR), with the date of that claim | No user is currently listed as assignee, and no comment claiming the issue or linked open, unmerged PR is dated within 90 days of the reference date — a claim or assignment older than that does not count against the issue | required |
| Not a graveyard of abandoned attempts | full comment thread: every comment claiming the issue, and whether that claim was later dropped — including a bot-driven auto-unassignment message (e.g. a repo bot commenting that a claimant "has been unassigned ... for over N days," a stale-bot timeout, or any automated reclaim notice), an explicit withdrawal, or no further activity from that person; repo-facts `this issue:` line for the state and, if given, the closed date of each linked PR (when no closed date is given, use the date of the nearest comment-thread activity tied to that PR as a proxy) | Fewer than 3 distinct people have claimed the issue and then dropped it within the last 90 days — count a bot's auto-unassignment message as a dropped claim for the person it names — AND no linked PR against the issue was closed without being merged within the last 90 days; a closed-unmerged PR (or a claim-and-drop) older than 90 days does not count against the issue | required |
| Clear, specific description | issue body: presence of a concrete problem statement naming a specific, observable behavior (what happens, and under what condition) | Issue body names a specific, observable behavior — a concrete symptom or request tied to a condition or component, even in one or two sentences — rather than a vague title or an unspecific complaint with no identifiable behavior (e.g. "this is broken" or "please improve X" with no detail on what "broken" or "improve" means). Reproduction steps, expected-vs-actual phrasing, or a named file/function/line are each sufficient on their own but none is required if the problem statement is already concrete and specific | required |
| Repo does not forbid AI-assisted contributions | repo-facts block: the `contribution policy` line (drawn from CONTRIBUTING.md or the repo's stated policy) | Pass unless the policy text explicitly disallows AI-generated or AI-assisted code, documentation, or pull requests (e.g. "we do not accept AI-generated code," "AI tools are not permitted"). A policy that merely requires disclosure, review, or understanding of AI-assisted contributions (e.g. "you are responsible for reviewing AI-generated content") passes; only an outright ban fails | required |
| Scope fits a newcomer | issue body and thread: any size/effort signals — labels like "good first issue"/"help wanted", maintainer estimate of effort, or number of files/systems the fix likely touches as described | Issue is explicitly labeled "good first issue"/"help wanted", OR a maintainer comment estimates it as small/simple, OR the description implies a fix confined to a single file or function | preferred |
| Maintainer responsiveness beyond bare minimum | comment thread: time between the issue's opening and the maintainer's first substantive reply | Maintainer's first substantive reply came within 7 days of the issue being opened | preferred |

## Verdict rule

Accept only if every `required` check passes. Any `required` check that fails, or that comes back `unclear`, rejects the issue — `unclear` is treated as a fail, never as a pass. `preferred` checks never flip the verdict either way; they exist only to rank issues that already passed all required checks (more preferred checks satisfied = higher rank among accepted issues).


