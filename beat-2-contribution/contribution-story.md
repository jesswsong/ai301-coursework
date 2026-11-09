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

**Unit 6: Installed the voice guard, patched the rubric's scope check, set up repo memory**

I merged three weeks of voice guides into one canonical file, wrote the new "Upstream" rules (claims, questions, pings), and wired a PreToolUse hook that checks every outgoing `gh issue/pr comment`/`create` body against it before it posts. I pipe-tested it directly before trusting it live: a normal command passes through untouched, and a bodyless `gh issue comment --help` gets stopped exactly as designed — nothing broke there. What did fall short was my search process, not the tooling I built: I spent a lot of this week's time vetting candidates found through broad `gh search` (the scout's own search mechanism) that turned out not to exist in the course's actual Issue Selection tracker, which is a separate, smaller, fixed snapshot. I changed my process mid-week to cross-check every candidate against the tracker's own search box before investing time in it, and set up `CLAUDE.local.md` in my fork of `ROCm/iris` so the next session doesn't have to re-learn the subsystem (the GPU-runtime import quirk, which test pattern to follow) from scratch.

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

No AMD hardware available. `iris/host/platform/hip.py` loads a GPU vendor runtime library via `ctypes` at module-import time, so importing it at all needs at least that shared-library file present (no physical GPU required for that). I used a Docker container instead of real hardware: `python:3.11-slim`, Python 3.11.18, Linux 6.10.14-linuxkit-aarch64, `numpy` 2.4.6, `torch` 2.15.0+cpu, `nvidia-cuda-runtime-cu12` (ships `libcudart.so.12`, symlinked to the unversioned `libcudart.so` name `hip.py`'s fallback loader looks for).

I did not `import iris` / `pip install -e ".[dev]"`: the top-level `iris/__init__.py` pulls in `triton` and a git-sourced `tritonBLAS` dependency, neither needed to exercise one caching function and both adding setup risk without adding evidence. `repro_isolated_hip.py` instead loads `iris/host/platform/hip.py` directly via `importlib.util.spec_from_file_location`, pre-registering empty stub packages in `sys.modules` for `iris`, `iris.host`, `iris.host.logging`, `iris.host.platform` (the parent packages `hip.py`'s own imports need present), and loading the real `iris/host/platform/libs.py` and `iris/host/logging/logging.py` for real (both stdlib-only, no heavy deps).

Repo commit tested: `ROCm/iris@17226794ea885dde60072683b3522e4d149ca8ab` (fork `jesswsong/iris`, `main`, post-PR #595 merge).

**Exact commands**, self-contained — run from any empty working directory. They create their own `repro-590/` folder and clone the repo; `repro_isolated_hip.py`'s full, unabridged contents are pasted below the command block — save that as `repro_isolated_hip.py` inside `repro-590/`, next to (not inside) the `iris/` clone, before running `docker run`:

```bash
mkdir -p repro-590 && cd repro-590
git clone https://github.com/jesswsong/iris.git

docker run --rm \
  -v "$(pwd)/iris":/workspace \
  -v "$(pwd)/repro_isolated_hip.py":/repro_isolated_hip.py \
  -w /workspace python:3.11-slim bash -c '
set -e
pip install -q numpy nvidia-cuda-runtime-cu12
pip install -q torch --index-url https://download.pytorch.org/whl/cpu
LIBFILE=$(find / -name "libcudart.so.*" 2>/dev/null | head -1)
LIBDIR=$(dirname "$LIBFILE")
ln -sf "$LIBFILE" "$LIBDIR"/libcudart.so
export LD_LIBRARY_PATH="$LIBDIR":$LD_LIBRARY_PATH
python3 /repro_isolated_hip.py
'
```

`IRIS_REPO_PATH` (read by `repro_isolated_hip.py`, default `/workspace`, which is what the bind-mount above maps to) can be set to point at a different checkout layout than the one above.

**`repro_isolated_hip.py`**, full contents — save exactly this as `repro-590/repro_isolated_hip.py`:

```python
"""
Reproduction script for ROCm/iris#590: get_num_xcc() is not cached.

Loads iris/host/platform/hip.py in isolation (stubbing the heavy parent
`iris` package, which pulls in triton/tritonblas that aren't needed to
reproduce this specific caching bug) and shows that, with
get_rocm_version() mocked to avoid any real subprocess/ctypes calls,
get_num_xcc() still re-invokes the underlying device-attribute lookup
on every call, while get_rocm_version() (fixed by PR #595) does not.
"""

import importlib.util
import os
import sys
import types
from pathlib import Path
from unittest import mock

# Path to a checkout of ROCm/iris (or your fork). The exact command
# block in repro-draft.md's "Environment Setup" section bind-mounts
# the repo to /workspace inside the container; set IRIS_REPO_PATH to
# override for a different layout.
REPO = Path(os.environ.get("IRIS_REPO_PATH", "/workspace"))

for name in [
    "iris",
    "iris.host",
    "iris.host.logging",
    "iris.host.platform",
]:
    sys.modules[name] = types.ModuleType(name)


def load_real_module(dotted_name: str, path: Path):
    spec = importlib.util.spec_from_file_location(dotted_name, path)
    module = importlib.util.module_from_spec(spec)
    sys.modules[dotted_name] = module
    spec.loader.exec_module(module)
    return module


load_real_module("iris.host.logging.logging", REPO / "iris/host/logging/logging.py")
load_real_module("iris.host.platform.libs", REPO / "iris/host/platform/libs.py")
hip = load_real_module("iris.host.platform.hip", REPO / "iris/host/platform/hip.py")

print(f"Imported hip.py OK. _is_amd_backend = {hip._is_amd_backend}")
print(f"get_rocm_version has lru_cache: {hasattr(hip.get_rocm_version, 'cache_info')}")
print(f"get_num_xcc has lru_cache: {hasattr(hip.get_num_xcc, 'cache_info')}")
print()

# get_rocm_version() is already cached (PR #595) -- force the AMD code
# path (this sandbox only has the CUDA runtime stub loaded) and confirm
# subprocess runs once across 5 calls.
hip.get_rocm_version.cache_clear()
with mock.patch.object(hip, "_is_amd_backend", True), mock.patch.object(
    hip.subprocess, "run", return_value=mock.Mock(stdout="/opt/rocm\n")
) as run, mock.patch("builtins.open", mock.mock_open(read_data="7.1.0-12345\n")):
    for _ in range(5):
        hip.get_rocm_version()
    print(f"get_rocm_version(): subprocess.run call_count over 5 calls = {run.call_count} (expected 1, cached)")

# get_num_xcc() is NOT cached -- show the underlying device-attribute
# lookup fires on every call even though get_rocm_version() underneath
# it is now cheap. Replace gpu_runtime wholesale (a mock, not the real
# CDLL) since the real object has no hip* symbols with only libcudart
# loaded in this sandbox.
hip.get_rocm_version.cache_clear()
mock_gpu_runtime = mock.MagicMock()
mock_gpu_runtime.hipDeviceGetAttribute.return_value = 0
with mock.patch.object(hip, "_is_amd_backend", True), mock.patch.object(
    hip, "get_rocm_version", return_value=(7, 1)
), mock.patch.object(hip, "get_device_id", return_value=0), mock.patch.object(
    hip, "gpu_runtime", mock_gpu_runtime
), mock.patch.object(
    hip, "gpu_try"
):
    for _ in range(5):
        hip.get_num_xcc(device_id=0)
    print(
        f"get_num_xcc(device_id=0): hipDeviceGetAttribute call_count over 5 calls = "
        f"{mock_gpu_runtime.hipDeviceGetAttribute.call_count} (not cached -- fires every call)"
    )
```

**Steps to Reproduce**

1. Run the exact command block above.
2. It imports `hip.py` in isolation and prints whether each function carries `functools.lru_cache`.
3. It then calls `get_rocm_version()` 5 times with `subprocess.run`/`open()` mocked, and `get_num_xcc(device_id=0)` 5 times with `get_rocm_version`, `get_device_id`, and `gpu_runtime` mocked (forcing `hip._is_amd_backend = True` first, since this sandbox only has the CUDA runtime stub loaded, not an AMD one) — and prints each mocked dependency's call count.

**Reproduction Evidence**

- **Commit showing reproduction:** none — this is a read-only repro script, not a fix.
- **Script:** `repro_isolated_hip.py` (this folder), run via the command above.
- **Output:**
  ```
  Imported hip.py OK. _is_amd_backend = False
  get_rocm_version has lru_cache: True
  get_num_xcc has lru_cache: False

  get_rocm_version(): subprocess.run call_count over 5 calls = 1 (expected 1, cached)
  get_num_xcc(device_id=0): hipDeviceGetAttribute call_count over 5 calls = 5 (not cached -- fires every call)
  ```
- **My findings:** `get_rocm_version()` already carries `functools.lru_cache` (confirms PR #595 is in place; its mocked `subprocess.run` is called once across 5 invocations, not part of what I'm claiming here). `get_num_xcc()` has no cache: the call count on its mocked dependency (`hipDeviceGetAttribute`) is 5 for 5 calls. I did not measure real timing or cost here — only presence/absence of caching, with every underlying call mocked. My claim comment already treats the real-world cost of this as likely small (microseconds, per PR #595's author); a real cost measurement would need an actual ROCm machine, which I don't have, so I'm scoping this report to the missing cache, not a performance claim.

**Toolkit**

- Tool: repro-check, run in live mode against `claim-draft.md` and this report together before posting either.
- What it produced: two rejects before an accept. Round 1 flagged prose-only setup steps, a reproduced-symptom claim that contradicted my own output (PR #595 already fixed `get_rocm_version()`), and an over-claimed "matches the issue exactly." Round 2 flagged that the script itself was unreachable to a stranger reading the posted comment (referenced a "companion file" that wasn't tracked anywhere).
- Difficulties: getting the wording precise enough that the report claims only what the mocked evidence actually shows (missing cache) and not what it can't show (real-world cost) took two rounds to get right.
- What changed: rewrote the setup as exact, self-contained commands; reworded the finding to the literal call-count claim above; and pasted the script's full contents inline in this report instead of pointing at an untracked file, so a stranger can run it from the comment alone.

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
