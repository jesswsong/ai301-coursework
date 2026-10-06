# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diagnosis matches reproduction | The plan's diagnosis and explanation of the cause, read against the reproduction evidence's steps, observed result, and expected result in the package. | The plan identifies a cause that accounts for the behavior shown by the reproduction evidence and explains the connection. Merely restating that the problem occurs, or proposing a cause contradicted by the reproduction, does not pass. | required |
| Specific, bounded scope | The plan's description of where the bug occurs and what will change, read against the issue context and the reproduction evidence. | The plan identifies the affected location precisely enough to find it (for example, a file and function or relevant line range) and limits the proposed change to the behavior supported by the issue and reproduction. A vague location or unrelated scope expansion does not pass. | required |
| Reproducible tests and expected outcome | The plan's test instructions and expected results, read against the reproduction evidence's steps and the behavior the plan proposes to fix. | A contributor can follow the stated steps or run the named test command(s) to exercise the reproduced behavior, and the plan states an observable expected result that demonstrates the bug is fixed. Generic instructions to test, or tests without an expected result tied to the reproduction, do not pass. | required |

## Verdict rule

Accept (ready) only if every required check passes. Reject (hold) if
any required check fails or is unclear (`?`). Preferred checks, if
added, never change the verdict.
