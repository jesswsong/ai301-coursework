# Evidence guide: where evidence lives in a plan package

Use this guide to locate and interpret evidence named by `rubric.md`.
It is a map, not an extra set of checks: grade only checks in the
rubric, apply their stated pass conditions, and do not award a pass
solely because a plan contains a particular heading or amount of text.
For every grade, record the specific fact or short quote that supports
it.

## Evidence boundaries and source map

### Eval mode

The eval bundle is the entire evidence universe. Find evidence in the
sections labeled for the issue context, repo facts, repro evidence,
candidate plan, and candidate plan comment. Use only the bundle text:
do not fetch the repository, issue, links, or other files, and do not
fill gaps using outside knowledge. If a needed fact is absent, treat
that absence according to the rubric and procedure (normally `unclear`
when the check cannot be determined).

### Live mode

Use the scoped GitHub issue and repository, following `scope.md`.
Gather:

- **Issue context:** the issue title and body, including its stated
  expected behavior, actual behavior, constraints, and links that are
  relevant to understanding the report.
- **Issue-thread signals:** maintainer and contributor comments that
  clarify reproduction, intended behavior, affected versions, accepted
  approaches, constraints, or requested coordination. Distinguish
  maintainer guidance from speculation; apply any classroom house rule
  in `scope.md`.
- **Reproduction evidence:** the student's own posted reproduction
  comment on the issue. Extract the environment/version, setup and
  actions, observed result, and expected result. For a designated
  house issue with no student repro comment, use only the house repro
  pack quoted in the candidate drafts; if the drafts quote none, do
  not substitute a classmate's repro or an unposted local report.
- **Repository facts:** relevant repository files such as
  `CONTRIBUTING.md`, contributor or testing documentation,
  `package.json` or equivalent build configuration, and the
  implementation or test files implicated by the issue. Use these to
  confirm conventions and whether proposed paths and commands are
  plausible—not to silently expand what the candidate plan says.
- **Candidate package:** the student's draft plan and draft plan
  comment. Judge what would be visible in the posted comment, including
  any repro details it quotes, rather than assuming an unmentioned
  local file will accompany it.

Prefer primary evidence: issue text and maintainer statements for
reported intent, the student's repro for demonstrated behavior, and
repository files or test output for code and test conventions. If
sources conflict, record the conflict and do not present an inference
as an established fact.

## Diagnosis and grounding

**Where to look**

- In the issue context, locate the reported symptom, conditions, and
  expected behavior.
- In the repro evidence, locate the exact setup, actions, observed
  result, and expected result.
- In the candidate plan, locate its explanation of the cause and the
  reasoning that connects that cause to the observed behavior.
- In live mode, consult relevant maintainer clarifications and the
  implicated source or tests only when they are available and needed
  to check a factual claim. In eval mode, use only quoted bundle
  evidence.

**What to record and how to interpret it**

Write down the behavior the reproduction establishes, then compare the
plan's proposed cause with that behavior. A diagnosis is grounded when
the described cause plausibly explains the observed result under the
reproduction's conditions and is consistent with the expected result.
The plan need not prove an unverified internal mechanism; it should
identify uncertainty rather than state a guess as fact.

**Examples**

- Grounded: the repro shows a valid empty list is treated as missing;
  the plan points to a truthiness check in the input handling path and
  proposes distinguishing “not provided” from “provided but empty.”
- Not grounded: the repro shows the issue only after a request times
  out, but the plan attributes it to rendering without explaining how
  rendering causes that timeout.
- Insufficient: the plan repeats “the command crashes” but gives no
  cause or connection to the repro. A matching symptom alone is not a
  diagnosis.

Do not infer that a cause is correct merely because the plan names a
plausible file or because a proposed change might suppress the
symptom.

## Scope

**Where to look**

- In the candidate plan, find what behavior it will change, where the
  bug occurs, what files/functions/components are likely involved, and
  any explicit boundaries or exclusions.
- Compare those statements with the issue context and repro steps.
- In live mode, inspect the relevant repository paths only as needed
  to check that the named location is findable and corresponds to the
  described behavior. Repository inspection can corroborate location;
  it does not add unstated work to the plan.

**What to record and how to interpret it**

Record a locator precise enough for a contributor to find the likely
change site: for example, a path plus function, class, component, or
relevant line range. Line numbers can shift; a stable symbol or path
and behavior is usually more useful than a line number alone. Record
the plan's intended change and compare its breadth with the issue and
repro. A bounded scope addresses the demonstrated behavior and avoids
unrelated cleanup or redesign.

**Examples**

- Specific and bounded: “In `src/parser.ts`, update `parseOptions` so
  an explicitly empty option is preserved; do not change config-file
  precedence.”
- Too vague: “Fix the parser.”
- Overbroad: the issue concerns one invalid default, while the plan
  proposes replacing the configuration system and reformatting every
  caller without evidence those changes are needed.

Do not require exact line numbers if a stable function or component
clearly locates the code. Do not treat a list of many files as
automatically overbroad; assess whether each area is connected to the
reproduced behavior.

## Executability

**Where to look**

- In the candidate plan and plan comment, find the described approach,
  intended code or test locations, and any sequencing or decision
  points.
- In live mode, consult repository structure, contribution docs, and
  relevant source/tests to determine whether the first action is
  findable and consistent with the project. In eval mode, use a
  repo-facts block only if the bundle provides one.

**What to record and how to interpret it**

Ask whether a contributor who has not spoken with the plan's author
can start the first meaningful step and understand what result to
check. A plan may leave implementation details open when exploration
is genuinely required, but it should name the question to resolve and
how the answer affects the next step. Record blockers or unsupported
assumptions rather than inventing missing instructions.

**Examples**

- Actionable: “Add a regression case beside the existing
  `parseOptions` tests, reproduce the empty-value failure, then update
  the parser branch that currently drops it.”
- Not actionable: “Investigate and fix parsing,” with no clue which
  behavior, test, or code path to investigate.
- A reasonable open question: “Check whether the CLI and config-file
  paths share this parser; if they do, cover both callers, otherwise
  keep the change in the CLI path.” The plan identifies the uncertainty
  and how to resolve it.

Do not grade plans on length or headings. Grade whether their proposed
work can be started and followed from the information actually
provided.

## Test plan

**Where to look**

- In the reproduction evidence, identify the setup, exact actions,
  observed result, and expected result that define the regression.
- In the candidate plan, locate test commands or repeatable steps,
  proposed regression-test location or level, and the expected
  observable result after the fix.
- In live mode, use repository test documentation, CI configuration,
  and neighboring tests to verify that named commands and test
  locations exist or are plausible. Do not run commands unless the
  task asks for execution. In eval mode, rely only on bundle content.

**What to record and how to interpret it**

Record whether the test exercises the same conditions as the repro,
what assertion or visible result distinguishes fixed from broken, and
how to run it. A useful test plan makes failure before the fix and
success after it observable. If the project has no automated test for
the behavior, a precise manual verification may still be reproducible;
the plan should identify actions and expected output, not merely say
“test manually.”

**Examples**

- Reproducible: “Run `npm test -- --runInBand test/parser.test.ts`;
  add a case with `--label=` and assert the
  parsed label is the empty string. It currently becomes `undefined`;
  after the fix the assertion passes.”
- Reproducible manual check: “With version X and the sample config,
  run these two commands; the first should report the empty value and
  the second should still report the default when the option is
  omitted.”
- Not decisive: “Run the tests” without naming a command or the
  relevant behavior.
- No expected outcome: a command is named, but the plan never says
  what result demonstrates the reproduced bug is fixed.

Do not require a particular test framework or insist on unit tests
when an integration or manual check is more appropriate. Require a
clear route from the repro to an observable pass condition.

## Honesty

**Where to look**

- In the candidate plan and plan comment, identify statements about
  confidence, assumptions, risks, alternatives, and unknowns.
- For a plan updated after implementation, locate its deviation note
  and compare the stated change and reason with the original plan and
  available evidence.
- In live mode, compare factual claims with the issue thread and
  repository evidence. In eval mode, use only the bundle.

**What to record and how to interpret it**

Separate observed facts from hypotheses. An honest plan can state a
likely cause while marking it as a hypothesis and specifying how to
verify it. A material unknown is not a flaw by itself; presenting an
untested assumption as confirmed is. For a deviation, look for what
changed and why, not merely a claim that the implementation “needed”
to differ.

**Examples**

- Honest uncertainty: “The repro suggests the cache key omits the
  locale; confirm by logging the key for both requests before changing
  invalidation.”
- Unsupported certainty: “The cache key is definitely wrong,” when
  neither the repro nor cited repository evidence establishes that.
- Honest deviation: “The shared helper also serves the config path, so
  the fix now covers both callers; added a regression case for each.”

Report uncertainty with its evidence; do not convert a plausible
hypothesis into a confirmed diagnosis.

## Comms

**Where to look**

- Read the candidate plan comment as it would appear on the issue.
- In live mode, compare it with relevant maintainer comments, issue
  instructions, and repository contribution guidance. Check
  `scope.md` for Path Review house rules.
- In eval mode, use only the thread highlights and repo-facts block in
  the bundle; do not fetch the issue or repository.
- In both modes, consult `voice-guide.md` for live-mode writing
  feedback when directed by `SKILL.md`; the voice guide does not add a
  rubric check or independently change the verdict.

**What to record and how to interpret it**

Record any explicit maintainer request, constraint, unanswered
question, contribution template, test instruction, or required
disclosure that is relevant to the draft. Check whether the comment
responds to those signals and accurately summarizes the candidate's
own plan. Apply the scope's house rules even when they differ from
usual open-source practice.

**Examples**

- Thread-aware: a maintainer asks for a regression test and the plan
  comment names the test the student intends to add.
- Boilerplate: the comment says “I will fix this” but omits a
  maintainer's explicit request to first confirm behavior on the
  supported version.
- House-rule aware: where `scope.md` says classmates' plans do not
  block the student, the presence of a classmate's plan is not evidence
  that the candidate must withdraw or wait.

Do not treat silence as approval, infer maintainer intent from a
classmate's speculation, or penalize a draft for requirements absent
from the permitted evidence.
