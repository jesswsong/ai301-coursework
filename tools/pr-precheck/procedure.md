# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

## Read order

1. In live mode, read `scope.md` first. Stop without grading if the
   `Repo:` line is blank/placeholder or the PR targets a different
   repository; note the applicable Path Review house rules. Ignore
   `scope.md` in eval mode.
2. Read `rubric.md` and `references/evidence-guide.md`. List the
   checks in table order, their weights, evidence sources, pass
   conditions, and verdict rule. If there are no written checks or no
   written procedure steps, refuse to grade.
3. In eval mode, read the single bundle from beginning to end and use
   nothing outside it. In live mode, read `plan.md` (or the house plan
   and house repro pack), including all deviation notes, before
   inspecting implementation evidence. Then read `pr_draft.md` for
   the title, description, and issue reference, and `test_evidence.md`
   for the reported commands and results.
4. In live mode, run `git diff main...HEAD` from the repository
   working copy and read the complete diff and changed-file list.
   Read the referenced issue's body and thread comments, the
   repository PR template, and `docs/CONTRIBUTING.md`. Read
   `voice-guide.md` before judging
   the draft title and description. This order establishes the
   intended scope and claims before comparing them with the delivered
   diff and its proof.

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides where the plan sits in the order (before the diff? before the
description?) and says why the order matters for the side-by-side
checks that come later. -->

## Evidence gathering

Gather and record specific facts for every check; use the evidence
guide for the exact locations and good-evidence criteria.

1. **Plan fidelity:** Record the plan's diagnosis, in-scope and
   excluded work, named files, and any deviation note. Compare the
   diagnosis with the issue's reproduced behavior and the diff's
   implementation; compare the plan's boundaries with the diff's
   actual changed files and hunks. Then compare each material claim in
   the title and description with both the plan and the diff. Record
   unplanned additions/omissions and whether any deviation is
   explicitly documented and keeps the change coherent.
2. **Test evidence:** Extract the plan's test plan and the issue's
   triggering behavior/expected result. Compare them with the
   commands, before/after outputs, and outcomes in `test_evidence.md`
   (or the eval bundle's test-evidence section). Separately record
   whether applicable required checks from the template or
   contribution instructions have reported outcomes.
3. **Diff quality:** List the changed files and inspect the diff
   hunks and commits for the intended fix, unrelated edits, debug
   leftovers, dead/commented-out code, and churn that could obscure
   review. Compare each changed area with the plan and issue.
4. **Standards and comms:** Extract concrete asks from the issue
   thread, PR template, and `docs/CONTRIBUTING.md` (or the eval
   repo-facts block). Check the title and description for real
   responses to applicable template fields, required disclosures, and
   explicit maintainer direction. If a required source cannot be
   obtained, record precisely which one is unavailable.
5. In eval mode, take every fact only from the bundle's issue,
   thread highlights, repo facts, plan context, candidate title and
   description, commits, diff, and test-evidence sections. Do not
   fetch or supplement anything. In live mode, use only the listed
   working-copy files, command output, issue thread, and repository
   standards for the corresponding facts.

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which file, diff,
or page per your evidence guide) to pull the fact from, and what to
record. The load-bearing gathers this week are pairings: diff files
against plan scope, claimed evidence against the plan's test plan and
the repo's checks, description claims against diff contents. A
complete procedure leaves no check whose evidence an executor would
have to hunt for. -->

## Check execution

Grade in rubric table order, applying each pass condition to its
named evidence:

1. For **Plan fidelity**, pass only when the diff implements the
   plan's approach to the behavior shown by the issue/reproduction,
   and the diff and its claims match the plan, or a concrete deviation
   is recorded honestly and the actual change remains coherent. A
   symptom-only change that misses the plan's stated cause, a
   deviation visible only in the diff, an unacknowledged shortfall, or
   a title/description claim contradicted by the diff fails.
2. For **Test evidence**, pass only when the reported results make
   the planned/reproduced behavior observable and applicable required
   checks have evidence of their outcome. Do not treat a test command
   without a result as a passing test. Grade a disclosed shortfall
   using the rubric's stated exception, not as an automatic failure.
3. For **Diff quality**, pass only when the relevant implementation
   can be reviewed without unrelated edits or debris materially
   hiding it. Do not infer quality from the description or commit
   titles; inspect the diff.
4. For **Standards and comms**, pass only when concrete applicable
   requests are followed and the draft responds to explicit thread
   direction. Do not infer a disclosure requirement unless the
   policy/template asks for one. Boilerplate that does not answer a
   required ask fails.
5. Assign `unclear` only if evidence needed by that check is
   genuinely missing or its permitted source is unavailable. Do not
   use `unclear` for a negative fact that can be observed; grade that
   `fail`. Give every grade one concise, decisive evidence fact or
   quote. Grade each check independently; a pass on one cannot offset
   a failure on another.
6. In live mode, separately compare the title and description with
   `voice-guide.md`; identify every broken rule in the readable
   summary. This feedback changes no check grade or verdict unless a
   rubric check covers the same requirement. Eval mode does not read
   the voice guide.

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

## Verdict assembly

Apply the rubric's verdict rule after grading every check:
`accept` only if all required checks pass; otherwise `reject`,
including any required `unclear`. If several required checks fail or
are unclear, identify the first such check in rubric order as the
decisive reason in the readable summary. Include every check, grade,
and one-line deciding fact in the final JSON. Use only `accept` or
`reject`. End with the valid fenced JSON block required by
`SKILL.md`, with nothing after it.

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check: the
check whose failure the verdict turned on. When more than one check
failed, your procedure picks which one gets quoted (first failing
required check in rubric order is a fine rule); nothing picks it for
you, so write the rule down. A complete procedure produces the same
verdict from the same grades, every time. -->
