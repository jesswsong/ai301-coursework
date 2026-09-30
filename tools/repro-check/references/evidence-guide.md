# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

Where it lives:
- In a bundle, the issue context and repo-facts block usually establish the target repo, version, platform, toolchain, or configuration. The repro report's environment record is where the report names what it actually ran under.
- In live mode, look at the issue body, linked repo docs, the relevant version information in the issue thread, and the student's draft comment for the environment it claims to have tested.

What good looks like:
- The environment record names the exact repo, language/runtime, OS, package version, and relevant settings that matter to the issue, or it explicitly states where it differs from the issue's target environment.
- The environment should be specific enough that a stranger could tell whether the report is testing the same conditions as the issue and whether a mismatch explains a non-repro.

## Steps

Where it lives:
- In a bundle, the repro report's steps are the main source. The issue context may also describe the intended repro path or trigger conditions.
- In live mode, look at the candidate repro comment and the issue's own reproduction instructions or example workflow.

What good looks like:
- The steps start from a clear beginning state, tell the reader what to run or click, and end in the behavior being observed.
- A good report does not hide setup, assumes no prior local state, or leaves out a required prerequisite. Another person can follow the steps without guessing.

## Behavior shown

Where it lives:
- In a bundle, the behavior is usually in the repro report's pasted output, screenshots, logs, or terminal excerpts. The issue context tells you what behavior the issue claims to be.
- In live mode, look at the issue's bug description and the artifact quotes or screenshots attached in the draft comment.

What good looks like:
- The artifact directly shows the failure, warning, stack trace, UI behavior, or output that the issue is about. The same symptom appears in the issue description, or the report calls out a different bug's behavior instead of claiming the original issue.
- A strong report connects the observation to the issue: "the page throws X when Y is clicked" matches the described bug, while a nearby crash or an unrelated error does not.

## Honesty

Where it lives:
- In the claim comment and repro report, where the author says what they observed and what they believe it means.
- In the issue context and repo-facts block, where the target behavior is stated and where a mismatch can become visible.

What good looks like:
- The report either reproduces the issue with evidence or explicitly says it could not reproduce it and explains why the evidence is insufficient.
- Honesty means the wording stays within what the evidence supports: no stronger claim than "I observed X" or "I could not observe Y in this environment".

## Comms

Where it lives:
- In the claim comment against the issue thread, in the draft repro comment, and in any repo guidance or templates the issue references.
- In live mode, check the issue's repository conventions and the student's own voice guide for what they promised to write upstream.

What good looks like:
- Comments are direct, specific, and evidence-backed. They state the environment, the observed behavior, and the outcome without filler or exaggeration.
- The wording matches the repo's norms and does not rely on boilerplate such as "same issue" or "works for me" when the evidence does not support that conclusion.
