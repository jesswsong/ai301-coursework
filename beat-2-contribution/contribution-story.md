# Contribution: get_rocm_version() spawns hipconfig on every call, and get_num_xcc() calls it every time

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** jesswsong
**GitHub Username:** jesswsong
**Issue:** https://github.com/ROCm/iris/issues/590
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built the issue-scout and patched it after watching it fail**

I installed issue-scout, wrote a fit profile and three `gh search issues` queries in `search-strategy.md`, and ran two live scans. The first scan rejected all 5 candidates because my ranking filter's tie-break ("most recently updated") was rewarding hours-old, disposable repos that couldn't pass the maturity checks — I fixed it by dropping issues under 48 hours old and tie-breaking on oldest `createdAt` instead. The second scan then surfaced two accepted candidates on a 12-year-old repo, but manually checking it turned up a gap the rubric never caught: its last GitHub Release was from 2016 despite active commits, so I added a new required "release is reasonably recent" check to `rubric.md`.

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I found this issue by hand, not through a scout scan: I searched the course's AI301 Issue Selection tracker directly (filtering Language=Python, tags like `good first issue`/`help wanted`), since I learned this week that the scout's `gh search issues` queries pull from all of public GitHub, but my real submission has to come from that tracker's own smaller, fixed snapshot — several strong candidates I found through the scout's search machinery (`element-hq/synapse`, `sagemath/sage`, `astronomer/astronomer-cosmos`, `mila-iqia/cluv`) turned out not to be in the tracker at all, so they were never claimable. Once I found `ROCm/iris#590` in the tracker, I ran `issue-scout: grade` on it directly against my patched rubric.

The scout's verdict was **reject**, and I'm choosing it anyway. Every required check passed cleanly — maintainer active, repo active, scope-feasible, not a graveyard, clear description, no AI-contribution ban, nobody currently on it — except the "Release is reasonably recent" check I added in Unit 5, which graded `unclear` because `ROCm/iris` has never published a GitHub Release at all, despite being pushed to daily. That's the same false-negative I designed the check to flag honestly rather than silently miscall: a continuously-shipped internal/framework repo that just doesn't use GitHub's Release feature isn't the same thing as a stale one, and I'd rather accept this issue with eyes open than patch the rubric again under deadline pressure to rubber-stamp my own pick.

It fits my profile: pure Python, a concrete and bounded fix (add `functools.lru_cache` to `get_num_xcc()`, keyed by `device_id`), in an AMD-backed GPU/multi-GPU framework — a real resume-worthy project in the GPU/AI-infra space. What makes it genuinely mine to do, not just a copy of an already-known fix: a merged PR (#595) already cached the sibling function `get_rocm_version()`, and its author *deliberately declined* to cache `get_num_xcc()`, reasoning (without measuring) that what's left there is microseconds of work. I'm not taking that reasoning on faith — my claim comment asks the maintainer directly whether they still want it cached, which is the open design question I hope to learn from working through. I also had to dig in myself to confirm the local hardware blocker the prior contributor hit (no AMD GPU) doesn't actually block me: `hip.py` loads a GPU vendor runtime library at import time, so testing needs a Linux environment with at least the CUDA runtime's shared library installed (no physical GPU required) — a Docker container gets me there.

Honest read of the tool: the scout did its job on the checks it has, but it has two real blind spots I hit this week. First, its "nobody else is already on it" check reads the assignee field and claim comments, but doesn't walk the GitHub cross-reference timeline for PRs that may have *partially* addressed the issue — I found PR #595 by hand with a GraphQL query, not through the scout, and a verdict of "accept" on this issue without that context would have been misleading (I'd have thought I was doing fresh work when half of it was already done). Second, and more structurally: a real-world issue can be in a state the binary accept/reject can't express — "partially done, the remainder is a real but narrower question" is a third shape, not a clean accept or reject, and I didn't try to patch the rubric for it this week (logged as a gap in `scan-notes.md` instead, since a quick patch under deadline pressure felt riskier than naming the gap honestly).

Claim comment: https://github.com/ROCm/iris/issues/590#issuecomment-6731226113

**Toolkit**

- Tool: issue-scout (my search strategy, my rubric), run directly on a single issue URL rather than a live scan, since I'd already found the candidate by hand in the course tracker.
- What it produced: a reject verdict — every required check passed except "Release is reasonably recent," which came back `unclear` because this repo has never published a GitHub Release.
- Difficulties: the scout can't distinguish "stale" from "actively shipped but doesn't use Releases," and its claim-check doesn't search the PR cross-reference timeline for partial prior work, so it would have missed PR #595 entirely.
- What changed: I didn't patch the rubric further this week — the "partially addressed, real remainder" shape doesn't fit a binary accept/reject without more rework than I had time for, so I logged it as a known gap in `scan-notes.md` and overrode the verdict here in writing instead.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
