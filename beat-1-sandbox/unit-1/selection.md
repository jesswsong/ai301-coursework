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

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/22
[The individual Path Review issue page. A link to the repository or the issue list does not satisfy this field.]

**Verdict output**

[Your skill's live-mode output for this issue, pasted verbatim and ending with the
fenced JSON verdict block. A summary does not satisfy this field.]

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.


All three graded against `rubric.md`. Shared repo facts (reference date 2026-09-20): last default-branch commit 2026-09-16 by `Aburke225`, the repo's sole committer; all 5 most-recent commits (3× 2026-09-16, 2× 2026-08-24) within 90 days; repo not archived, 71 open issues, **zero pull requests in the entire repo**.

**Accepted, in fit order:**

**1. #22 — Add a mock GitHub API server for integration tests** — best fit: it's the only candidate that touches deployment-level plumbing (`pytest-httpserver` mock server, fixtures, and `.github/workflows/ci.yml`), and "I fixed a CI job that was passing on an empty test suite" is a real interview story. All 5 required checks pass.
- Maintainer active `pass` · Repo in use `pass` · Nobody on it `pass` (assignees `[]`, 0 comments, no linked PR) · Not a graveyard `pass` · Clear description `pass` (names 3 specific paths)
- Preferred: Scope `fail` — labeled `tier-2` (Intermediate difficulty), not `good first issue`, self-estimated 5–7 hours across 3 paths. Responsiveness `unclear`.
- Two things to know: the timeline shows `referenced` commit events from **MatthewOscar** on 2026-09-15 and 2026-09-19 — a classmate appears to be working it. Under the Path Review house rule that doesn't block you, and the rubric's claim condition (assignee / comment claim / open linked PR) is untouched by commit references, so it passes as written. If you'd rather not race, take #57.

**2. #57 — Tech detector counts vendored and build-output files** — Python, `good first issue` + `tier-1`, and the fix is real exclusion logic (not a one-liner) with two named failing tests to drive it. All 5 required pass; Scope `pass`, Responsiveness `unclear`.

**3. #60 — Faithfulness checker crashes on `text: None`** — cleanest and safest, but the smallest story: it's essentially `chunk.get("text") or ""`. All 5 required pass; Scope `pass`, Responsiveness `unclear`.

One tension worth noting for your rubric, not this run: "Maintainer responsiveness" can never pass on this repo, because no issue here has any comments — the check is dead weight in a classroom repo. And "Nobody else is already on it" reads only assignees, comments, and linked PRs, so it missed the commit references on #22 that are the strongest signal anyone is actually working.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/22",
    "checks": [
      {"name": "Maintainer is active", "grade": "pass", "evidence": "Last default-branch commit by Aburke225 on 2026-09-16, 4 days before the 2026-09-20 reference date"},
      {"name": "Repo is in active use", "grade": "pass", "evidence": "All 5 latest default-branch commits (3x 2026-09-16, 2x 2026-08-24) fall within 90 days; repo archived=false"},
      {"name": "Nobody else is already on it", "grade": "pass", "evidence": "assignees: [] and comments count 0; repo-wide PR list is an empty array, so no open linked PR"},
      {"name": "Not a graveyard of abandoned attempts", "grade": "pass", "evidence": "Zero claim comments and zero pull requests exist in the repo, so no dropped claims and no closed-unmerged linked PR"},
      {"name": "Clear, specific description", "grade": "pass", "evidence": "States the problem ('tests/integration/ holds only an __init__.py') and names files: tests/integration/test_github_tool.py, tests/fixtures/github_responses/, .github/workflows/ci.yml"},
      {"name": "Scope fits a newcomer", "grade": "fail", "evidence": "Labeled tier-2 (Intermediate difficulty), no good-first-issue/help-wanted label, 'Estimated effort: 5-7 hours' across 3 separate paths"},
      {"name": "Maintainer responsiveness beyond bare minimum", "grade": "unclear", "evidence": "Comments API returns an empty array, so there is no first-reply latency to measure"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/57",
    "checks": [
      {"name": "Maintainer is active", "grade": "pass", "evidence": "Last default-branch commit by Aburke225 on 2026-09-16, 4 days before the 2026-09-20 reference date"},
      {"name": "Repo is in active use", "grade": "pass", "evidence": "All 5 latest default-branch commits (3x 2026-09-16, 2x 2026-08-24) fall within 90 days; repo archived=false"},
      {"name": "Nobody else is already on it", "grade": "pass", "evidence": "assignees: [], comments count 0, Development box shows no branches or pull requests"},
      {"name": "Not a graveyard of abandoned attempts", "grade": "pass", "evidence": "Timeline holds only 4 labeled events by Aburke225; no claims, no linked PRs, zero PRs in the repo"},
      {"name": "Clear, specific description", "grade": "pass", "evidence": "Gives a runnable repro with expected vs observed (\"observed: 'JavaScript' (expected: 'Python')\") and names agent/tools/tech_detector.py plus tests test_node_modules_excluded and test_build_directory_excluded"},
      {"name": "Scope fits a newcomer", "grade": "pass", "evidence": "Labeled 'good first issue' and 'tier-1'; fix is confined to exclusion logic in tech_detector.py"},
      {"name": "Maintainer responsiveness beyond bare minimum", "grade": "unclear", "evidence": "Comments API returns an empty array, so there is no first-reply latency to measure"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60",
    "checks": [
      {"name": "Maintainer is active", "grade": "pass", "evidence": "Last default-branch commit by Aburke225 on 2026-09-16, 4 days before the 2026-09-20 reference date"},
      {"name": "Repo is in active use", "grade": "pass", "evidence": "All 5 latest default-branch commits (3x 2026-09-16, 2x 2026-08-24) fall within 90 days; repo archived=false"},
      {"name": "Nobody else is already on it", "grade": "pass", "evidence": "assignees: [], comments count 0, Development box reads 'No branches or pull requests'"},
      {"name": "Not a graveyard of abandoned attempts", "grade": "pass", "evidence": "Timeline holds only 4 labeled events by Aburke225; no claims, no linked PRs, zero PRs in the repo"},
      {"name": "Clear, specific description", "grade": "pass", "evidence": "Names the defect in check() ('chunk.get(\"text\", \"\")' returns None) with a one-line repro and the failing test tests/unit/test_faithfulness_checker.py::test_none_context_chunk_text"},
      {"name": "Scope fits a newcomer", "grade": "pass", "evidence": "Labeled 'good first issue' and 'tier-1'; fix is a single .get() call inside check()"},
      {"name": "Maintainer responsiveness beyond bare minimum", "grade": "unclear", "evidence": "Comments API returns an empty array, so there is no first-reply latency to measure"}
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

Several runs occurred while revising the rubric. Agreement scores, in order: 17/20 → 17/20 → **18/20**, the final run, matching the committed `eval-run.txt`:

```
categories: claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1  scope 2/4
agreement: 18/20 scored items  (bar: 18/20: PASS)
```


**Issue analysis**

`issue-12` — gold label `reject`, my rubric's final verdict `reject`, agreeing. Before
the last revision, this issue graded `accept` in more than one run. From
`eval/issues/issue-12.md` (bookwyrm-social/bookwyrm#1133), the repo-facts block states
the contribution policy plainly:

```
- contribution policy (CONTRIBUTING.md -> docs.joinbookwyrm.com/contributing.html, section "Generative AI"): "Meaningful human interaction is the whole point of BookWyrm. We do not accept AI-generated code or documentation. If you are unsure how something in BookWyrm works, please ask for help – we are keen to help other humans to understand and contribute to the project."
```

No check in my rubric read this field at all until the last revision, so whether
`issue-12` got rejected for this reason was pure luck — the model sometimes noticed the
policy unprompted and sometimes didn't. The gold `reject` reflects that an outright ban
on AI-generated contributions should sink an issue regardless of every other check,
since the course workflow is AI-assisted by design.

**Check rationale**

From `rubric.md`, the check I added:

```
| Repo does not forbid AI-assisted contributions | repo-facts block: the `contribution policy` line (drawn from CONTRIBUTING.md or the repo's stated policy) | Pass unless the policy text explicitly disallows AI-generated or AI-assisted code, documentation, or pull requests (e.g. "we do not accept AI-generated code," "AI tools are not permitted"). A policy that merely requires disclosure, review, or understanding of AI-assisted contributions (e.g. "you are responsible for reviewing AI-generated content") passes; only an outright ban fails | required |
```

I made this `required`, not `preferred`, because it isn't a nice-to-have about issue
quality — it's a hard eligibility gate. If the repo will not accept AI-generated work at
all, no amount of a good description or an active maintainer makes the issue winnable
through this workflow. I distinguished an outright ban from a mere disclosure/review
requirement (like conda's "you are responsible for all contributions and must review and
understand AI-generated content," which is not a ban) because treating every AI-adjacent
policy line as disqualifying would reject far more issues than the family is meant to
catch — the eval's `policy` category tracks exactly this distinction, and conflating the
two would have traded one miscalibration for another.

**Trade-offs**

This check trusts a coarse read of a single free-text policy line, so it will miss a ban
phrased less directly than bookwyrm's — e.g. a policy that discourages AI tools by
implication ("we value contributors who deeply understand their changes") without ever
saying "AI" or "generated" — and pass it as merely a disclosure requirement. I accept that
gap for now: the eval's `policy` category went from untested (no check existed) to `1/1`
on the one case it currently contains, and tightening the wording further without another
example of an indirect ban risks overfitting the check to language it will never see
again. Nothing else in the rubric changed to compensate for this narrower miss; it's an
accepted blind spot, not a covered one.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

[Answer all three:

1. The issue's fit to your interests and to the time available.
2. What the verdict identified correctly, and what you weighed that the rubric could not.
3. The anticipated difficulty in claiming it.]

This issue is a perfect fit to my interests. I'm looking for an issue that can help me add to my portfolio, gain more experience in creating deployment-level developments, and have more exposure to agent. The verdict correctly identified that it has an active maintainer, doesn't have anyone claiming it yet, and is not a graveyard issue. It couldn't identify exposure to the topic agent as one of my interests, which I weighed in choosing this issue. I think the difficulty will be just right - I have completed a tier-1 issue during AI201, so I want something more challenging than something that's good for a first issue. The anticipated hours to solve is 5-7 hours, which indiciate that it doesn't have the most complicated scope either. I think the difficulty and time required will be just the right challenge for me in terms of not being too hard but also will challenge my current skills and experience.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
