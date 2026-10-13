---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->

## The question

Grade exactly one pull request package to answer exactly one question:
is this ready to submit? A package consists of the plan it claims to
implement, the branch diff, the PR title and description, test
evidence, and the issue and repository standards the change belongs
to. Judge those artifacts against one another and the applicable
issue/repository evidence; do not grade an issue, plan, or code change
in isolation.

## Inputs and modes

Run in exactly one of these modes:

- **Live mode:** Read the student's `plan.md`, including its
  deviation notes; obtain the branch diff by running
  `git diff main...HEAD` from the repository working copy; read
  `test_evidence.md` and `pr_draft.md` (including the draft title and
  description); and identify the issue from the draft/package
  context. Read that issue's body and thread comments, the
  repository's PR template, and `docs/CONTRIBUTING.md`. A house-chain
  student uses the house plan and house reproduction pack in place
  of their personal plan and repro; the same checks still apply to
  their branch and PR.
  Follow `procedure.md` for the read order and evidence collection.
- **Eval mode:** Read one package bundle from `eval/packages/`. The
  bundle contains the issue and thread, repo facts, accepted-plan
  context, candidate PR, diff, and test evidence. Treat that bundle
  as the entire world: do not fetch pages or read local files. Grade
  every rubric check and apply the full verdict rule.

<!-- Define both modes. Live mode: name every input (your plan.md with
its deviation notes, your branch's diff, your draft PR title and
description, your test evidence, your issue), where each comes from,
and what a house-chain student reads instead. The branch's diff is
everything the branch changes relative to the repo's default branch:
`git diff main...HEAD` (three dots), run from the working copy, is
the command that produces it; name the source that concretely. Eval
mode: state that the bundle is the whole world, nothing is fetched,
and every check runs with the full verdict rule. -->

## The scope seam (live mode only)

Before reading or grading any live package, read `scope.md`. Confirm
the PR targets the repository named on its `Repo:` line and apply its
house rules. Refuse to grade if that line is blank or still contains a
placeholder, or if the PR targets another repository; state the reason
and do not guess a scope. Eval mode ignores `scope.md` entirely.

<!-- Tell the tool when to read scope.md, what to do with the rules it
finds there, what to refuse, and what to do when the scope's repo line
is an unfilled placeholder. State that eval mode ignores scope.md
entirely. CONTRACT.md names the required behavior; your frame has to
instruct it. -->

## The voice seam (live mode only)

In live mode, read `voice-guide.md` and compare its rules with the
outgoing PR title and description. Report each broken rule in the
readable summary, quoting or accurately identifying the rule. Voice
feedback does not change the verdict by itself unless a rubric check
also covers that requirement. Eval mode ignores `voice-guide.md`.

<!-- Tell the tool when to read voice-guide.md, which outgoing text it
gates (the PR title and description), how to report a broken rule, and
why it never changes the verdict on its own. State that eval mode
ignores it entirely. -->

## Component reads

Read `rubric.md` for the checks, their evidence and pass conditions,
weights, and verdict rule. Use `references/evidence-guide.md` to find
the evidence and understand what good evidence looks like. Execute
`procedure.md` as written to gather evidence, grade checks, and
assemble the verdict. If the procedure is silent on a necessary step,
report the gap rather than inventing a step. If the rubric has no
student-written checks or the procedure has no student-written steps,
refuse to grade and explain that the tool is incomplete. Template
comments do not count as rubric or procedure content.

<!-- Tell the tool how the pieces connect: rubric.md defines the
checks and the verdict rule, references/evidence-guide.md maps where
each evidence family lives, procedure.md is executed as written. Say
what the tool does when the procedure is silent on a step (report the
gap, never improvise around it) and what it does when rubric.md or
procedure.md has no content (the refusal rule, stated as an
instruction). -->

## Verdict and output

Use only the binary verdicts: `accept` means ready to submit;
`reject` means hold. End every graded reply with the fenced JSON block
below, valid and last, with nothing after it. Preserve its schema,
field order, and allowed values exactly.

<!-- State the binary verdict space (accept means submit, reject means
hold) and instruct the tool to end its reply with the fenced JSON
block below, valid and last, with nothing after it. The schema is
CONTRACT.md's, verbatim; do not edit it. -->

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

Name the fact or quote that decides every check; never grade from
impression. Grade the delivered change and evidence, not formatting or
polish. The rubric decides whether evidence passes, and the procedure
decides how to find and compare that evidence; report gaps instead of
silently filling them. Use `unclear` only when evidence required by a
check is genuinely missing or unavailable, not when you have not
looked. Treat `unclear` as a failure unless the rubric explicitly
specifies otherwise. An honestly documented deviation or shortfall
may still be ready when the rubric's conditions are met; do not reject
it merely for differing from the original plan.

<!-- Write the standing rules the tool grades under: evidence first,
grade the thing not the polish, the rubric decides, the procedure
decides how, and how unclear grades are treated when the rubric's
verdict rule is silent. CONTRACT.md states each as a guarantee; your
frame has to make them instructions. -->
