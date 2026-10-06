# Procedure: how this skill grades a plan package

## Read order

1. In live mode, read `scope.md` first. Confirm the issue belongs to
   the listed repository and note any Path Review house rules. If the
   repo line is a placeholder or the issue is outside that repository,
   stop without grading.
2. Read the issue context, then the student's reproduction evidence.
   Record the behavior at issue, the reproduction steps, the observed
   result, and the expected result. In eval mode, use only the issue
   context and repro-evidence block in the bundle.
3. Read the entire candidate plan and plan comment. Identify its
   proposed cause, affected location and change, and tests and expected
   outcomes. Grade only content in the drafts (and evidence they
   quote), not unposted material elsewhere in the student's working
   directory.
4. Read `rubric.md` and `references/evidence-guide.md`; list every
   check and the verdict rule. In live mode, also read
   `voice-guide.md` and note any writing rules the plan comment must
   follow. Reading the issue and reproduction first establishes the
   behavior against which the plan is judged.

## Evidence gathering

Gather the evidence named by each rubric row and record the specific
fact that supports the grade:

- **Diagnosis matches reproduction:** Record the plan's stated cause.
  Compare it with the repro evidence's steps, observed result, and
  expected result; note which behavior the proposed cause explains or
  contradicts.
- **Specific, bounded scope:** Record the location the plan names
  (file, function, line range, or other findable location), the
  proposed change, and any stated exclusions. Compare these with the
  issue context and reproduced behavior; note whether the location is
  findable and the change is limited to that behavior.
- **Reproducible tests and expected outcome:** Record the exact test
  command or actionable steps and the expected observable result.
  Compare them with the repro steps and the behavior the plan proposes
  to fix.

In an eval bundle, use only its issue-context, repro-evidence, and
candidate-plan content. In live mode, take issue-side facts from the
scoped issue and the student's own posted repro comment; take proposed
changes and tests from the candidate plan and plan comment. Apply the
Path Review house rules from `scope.md` when interpreting thread
evidence. Do not fill gaps with assumptions or unrelated files.

## Check execution

Grade the candidate plan against the gathered issue and reproduction
evidence, in the rubric's table order. For each check, assign `pass`,
`fail`, or `unclear` and give one concise evidence fact or quote:

1. **Diagnosis matches reproduction:** Pass only when the plan names a
   cause that accounts for the reproduced behavior and connects that
   cause to the evidence. Fail when it merely restates the symptom or
   proposes a cause contradicted by the evidence.
2. **Specific, bounded scope:** Pass only when the plan gives a
   findable location (such as a file and function or relevant line
   range) and confines the change to the issue and reproduced
   behavior. Fail when the location is too vague to find or the
   proposed change expands into unrelated work.
3. **Reproducible tests and expected outcome:** Pass only when a
   contributor can follow the stated steps or run named test
   command(s) to exercise the reproduced behavior, and the plan states
   an observable result that would demonstrate the fix works. Fail
   when the test instructions do not exercise that behavior or no
   relevant expected result is stated.

Use `unclear` only when required evidence is genuinely absent from the
package or unavailable in the permitted source; do not use it in place
of looking. Do not infer a missing cause, location, command, or
expected result. Grade each check from its named evidence without
letting a strong result on another check compensate for a failure.
In live mode, separately compare the draft plan comment with
`voice-guide.md` and report any broken rule in the summary; this does
not change the verdict unless a rubric check makes it relevant.

## Verdict assembly

Apply the verdict rule in `rubric.md` after grading every check:
accept only when all required checks pass; otherwise reject, including
when any required check is `unclear`. Preferred checks, if present,
never change the verdict. Include every check's name, grade, and
one-line deciding evidence in the output. The summary may identify the
required check that caused a rejection and include live-mode
voice-guide notes. Emit the required fenced JSON as the final content,
using `"unclear"` for a `?` grade and only `"accept"` or `"reject"`
for the verdict.
