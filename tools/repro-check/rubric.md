# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Environment is recorded | issue context, repo-facts block, and the repro report’s environment record; in live mode, also the issue thread or repo docs naming the target versions/platform | Pass if the report records the relevant version/platform/setup, or clearly states a difference from the issue’s target environment | required |
| Steps are complete | repro report steps and starting state, read against the issue’s described trigger | Pass if another person can follow the steps from a clean start to the same behavior without guessing or hidden setup | required |
| Behavior matches the issue | issue description and the report’s artifacts: output excerpts, logs, screenshots, terminal errors, stack traces | Pass if the artifact shows the same observable behavior the issue describes, not a nearby or unrelated problem | required |
| Outcome is honest | claim comment and the repro report’s stated result, read against the evidence shown | Pass if the report says only what it actually observed: reproduced, or honestly not reproduced | required |
| Repo conventions are respected | claim comment, repro report, and any repo instructions or templates the issue thread points to | Pass if the wording is specific, issue-focused, and consistent with the repo’s norms; it does not overstate confidence or claim more than the evidence supports | preferred |
| Repro is specific enough to judge | issue body and repro report: the problem statement, trigger condition, and observable symptom | Pass if the issue and report name a concrete symptom or condition that a stranger could recognize and verify | required |
| Evidence is not adjacent | issue description and the output/logs/screenshots in the report | Pass if the artifacts directly support the issue being reported rather than a different crash, unrelated warning, or a different product state | required |
| Claim is not over-claimed | claim comment and repro report against the evidence shown | Pass if the author does not claim a broader fix, broader regression, or stronger certainty than the evidence supports | required |
| Disclosure policy is satisfied | repo-facts block: the `contribution policy` line (drawn from CONTRIBUTING.md or the repo's stated policy), plus the claim comment and repro report if they mention AI use | Pass if the repo policy does not forbid AI-assisted contributions, or if it requires disclosure/review then the package clearly discloses AI use and confirms a human reviewed/edited the result. Fail if the policy bans AI-assisted work, or if a policy that requires disclosure/review is not satisfied by the package itself. | required |

## Verdict rule

Accept only if every required check passes. Any required check that fails, or that comes back unclear, rejects the package — unclear is treated as a fail, never as a pass. Preferred checks never flip the verdict either way; they exist only to rank issues that already passed all required checks (more preferred checks satisfied = higher rank among accepted packages).
