# Contribution: get_rocm_version() spawns hipconfig on every call, and get_num_xcc() calls it every time

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Jessica Song
**GitHub Username:** jesswsong
**Issue:** https://github.com/ROCm/iris/issues/590
**Pull Request:** [ROCm/iris#638](https://github.com/ROCm/iris/pull/638)

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

**Unit 7: Ship-gate checked the branch and caught release risks**

Ship-gate passed the manifest's Ruff checks and the live PR review on my branch. The first run could not read the issue because Anaconda's unrelated `gh` executable shadowed the real GitHub CLI; I put `/usr/local/bin` first in `~/.zshrc` and explicitly prioritized it for non-interactive gate runs. My ROCm reviewer's MI355X results later confirmed device switching, performance improvements, and passing pytest/distributed tests, so I updated the PR from a partial-scope description to `Fixes #590`, consistent with the course requirement and the review evidence. The review also caught that my original tests did not distinguish correct per-current-device caching from incorrectly caching the `None` default; I am adding a device-switch regression test before pushing the revision. The PR is [ROCm/iris#638](https://github.com/ROCm/iris/pull/638).

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

`get_num_xcc(device_id=None)` in `iris/host/platform/hip.py` recomputes the GPU's XCC (accelerator complex die) count from scratch on every call: it resolves `device_id` via `get_device_id()` when not given, reads `get_rocm_version()` to pick the attribute-lookup branch, and then makes one `hipDeviceGetAttribute` ctypes call into the vendor runtime. All of that work is wasted after the first call, because for a given device the installed ROCm version and that device's XCC count are both constants for the life of the process — they can't change between calls. PR #595 already fixed the sibling problem one layer down (`get_rocm_version()` used to spawn the `hipconfig` subprocess on every call) by caching it with `functools.lru_cache`, but deliberately left `get_num_xcc()` itself uncached, reasoning that what's left after #595 — `get_device_id()` plus one `hipDeviceGetAttribute` call — is microseconds of work not worth caching.

**Expected Behavior**

`get_num_xcc(device_id)` should do its real work (the device-ID resolution and the `hipDeviceGetAttribute` call) once per distinct device and reuse the result on every later call for that same device, the same way `get_rocm_version()` now does for the ROCm version.

**Current Behavior**

Every call to `get_num_xcc()`, even repeated calls for the same device, re-runs the full body: it re-resolves `device_id` (when not given) and re-issues the `hipDeviceGetAttribute` ctypes call. My reproduction confirms this directly: with the GPU runtime and its dependencies mocked, 5 calls to `get_num_xcc(device_id=0)` produce 5 calls to the mocked `hipDeviceGetAttribute`, not 1.

**Affected Components**

- `iris/host/platform/hip.py` — `get_num_xcc(device_id=None)` (line 217), the function to change.
- `iris/ccl/config.py` — `Config.__post_init__` (line 123-124) calls `iris.hip.get_num_xcc()` with no `device_id`, so it hits the `device_id is None` → `get_device_id()` path on every `Config()` construction where `num_xcds` isn't passed explicitly. This is the hot call site: a benchmark or framework integration that builds a `Config` per collective call pays the uncached cost every time.
- `iris/host/tracing/core.py` — calls `hip.get_num_xcc(self.iris.cur_rank)` with an explicit `device_id`, once per trace record.
- ~18 sites across `examples/`, mostly at module scope (paid once); `examples/08_gemm_all_reduce_atomics/matmul_wrapper.py:53` calls it inside a function.

**Toolkit**

- Tool: read directly — `gh issue view 590 --repo ROCm/iris` (and `--comments` for my own claim comment) for the issue text, then `grep`/`Read` over `iris/host/platform/hip.py`, `iris/ccl/config.py`, and `iris/host/tracing/core.py` to find every caller.
- What it produced: the exact line numbers and call patterns above, in particular that `Config.__post_init__` is the only caller that hits the `device_id=None` path, which is why the cache has to key on the *resolved* device, not on the literal `None` argument.
- Difficulties: none — this is close reading of a small, already-mapped file; no tool did anything surprising here.
- What changed: nothing yet; this section is the basis for the Analysis below.

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

Root cause: `get_num_xcc(device_id=None)` (`iris/host/platform/hip.py:217`) has no caching at all, unlike its sibling `get_rocm_version()` (same file, line 155), which PR #595 wrapped in `@functools.lru_cache(maxsize=None)`. The function's own body is pure given its argument — for AMD backends it does `get_device_id()` (only when `device_id is None`), a cheap `get_rocm_version()` lookup (already cached), and one `hipDeviceGetAttribute` ctypes call keyed on the resolved device id; for non-AMD backends it just returns `1`. None of that can change within a process for a given device.

The one subtlety, which the issue itself calls out ("either normalise the argument before caching or cache the inner body"): `device_id` defaults to `None`, and the *caller that matters most*, `Config.__post_init__` (`iris/ccl/config.py:123-124`), always calls `get_num_xcc()` with no argument. `get_device_id()` returns the *currently active* device (`hipGetDevice`/`cudaGetDevice`), which is not a fixed constant — it depends on which device the calling thread has selected (e.g. after `torch.cuda.set_device(...)`). If I put `@functools.lru_cache` directly on `get_num_xcc(device_id=None)`, the literal key for the hottest call site would always be `None`, so the *first* process-wide device would get cached under that key and every later call with `device_id=None` — even from a thread now on a different device — would silently return the first device's XCC count. That would be a correctness bug the issue's own checklist flags ("confirm no caller depends on re-reading either value after a device or environment change").

**Proposed Solution**

Resolve `device_id` to a concrete value in `get_num_xcc()` *before* the cache lookup (exactly as it already does today, just moved ahead of the cache), and delegate the actual lookup to a private `functools.lru_cache`-wrapped helper keyed on that concrete, resolved `device_id`. This caches correctly per real device no matter whether callers pass `device_id` explicitly or rely on the default, matches the pattern the issue asks for, and needs no change to any call site.

What I am deliberately **not** changing:
- `get_rocm_version()` — already fixed by PR #595, not mine to touch.
- Any call site (`Config.__post_init__`, `tracing/core.py`, the ~18 example sites) — the function's signature and return value are unchanged, so none of them need edits.
- The non-AMD (`_is_amd_backend is False`) short-circuit that returns `1` — still correct to cache, since it's also a per-process constant, and leaving it inside the cached helper keeps one code path instead of two.
- No timing/benchmark claim: like PR #595, I have no AMD hardware, so my evidence is a call-count assertion (mocked `hipDeviceGetAttribute`), not a wall-clock measurement. The issue's own cost numbers (median 15.6 ms for `hipconfig`, a separate call) don't transfer to this specific `hipDeviceGetAttribute` call, so I won't claim a saving in milliseconds I haven't measured.

**Implementation Plan**

1. In `iris/host/platform/hip.py`, split `get_num_xcc(device_id=None)` into:
   - a new private `@functools.lru_cache(maxsize=None)`-decorated function (e.g. `_get_num_xcc_cached(device_id)`) holding the existing body (the `_is_amd_backend` check, the `get_rocm_version()` branch, and the `hipDeviceGetAttribute` call), now always receiving a concrete `device_id`;
   - the public `get_num_xcc(device_id=None)` keeping only the `if device_id is None: device_id = get_device_id()` resolution, then returning `_get_num_xcc_cached(device_id)`.
2. Add `tests/unittests/test_get_num_xcc_cache.py`, following the exact pattern `tests/unittests/test_get_rocm_version_cache.py` uses for `get_rocm_version()`: mock `hip._is_amd_backend`, `hip.get_rocm_version`, `hip.get_device_id`, and `hip.gpu_runtime`/`hip.gpu_try`, call `get_num_xcc(device_id=0)` 5 times, and assert the mocked `hipDeviceGetAttribute` is called once, not 5 times. Also add a case calling `get_num_xcc()` (no argument) twice to confirm the `device_id=None` resolution path is cached too, and a case with two different `device_id`s to confirm they get distinct cache entries (not conflated).
3. Run `ruff check .` / `ruff format . --check` (my ship-manifest's checks) and the new test with plain `pytest` (no hardware needed, same as `test_get_rocm_version_cache.py`).
4. Update `CLAUDE.local.md`'s notes on `get_num_xcc` once the fix lands, and record anything that deviated from this plan here and in Implementation Notes below.

No deviations yet — this is the plan before the branch exists.

**Toolkit**

- Tool: plan-check (live mode), run on `plan.md` (a copy of the Analysis/Proposed Solution/Implementation Plan above) and `plan-comment.md` against `ROCm/iris#590`.
- What it produced: `accept` on the first run — all three required checks (diagnosis matches reproduction, specific/bounded scope, reproducible tests and expected outcome) passed.
- Difficulties: a real procedure gap, not a plan problem — my rubric's reproduction-evidence source is "the student's own posted repro comment on the issue," and I deliberately never posted one here (my earlier reproduction already matches what the issue itself documents in full; a duplicate comment would've added nothing new, which my own repro-check scope's wild rules explicitly warn against). The rubric's only written fallback for a missing posted comment is scoped to a "designated house issue," which this real issue isn't, so the letter of my own procedure didn't cover this case.
- What changed: I quoted the key repro evidence (the call-count result) directly into `plan-comment.md` and graded against that, the same way the scope's house-issue fallback treats a quoted repro pack. I didn't change the rubric or procedure itself this week — I'm naming the gap here rather than patching it under deadline pressure, the same call I made with the scout's "partially addressed" gap in Unit 5.

---

## Testing Strategy

Three observable checks, all recorded in `test_evidence.md`:

1. **The reproduction, re-run before and after.** The same isolated-import script from the Reproduction Process section, run against `main` and then against branch `jesswsong/cache-get-num-xcc` in the identical Docker/no-hardware setup. Before: `get_num_xcc(device_id=0)` calling the mocked `hipDeviceGetAttribute` 5 times over 5 calls. After: 1 time over 5 calls — the exact, named delta the fix is supposed to produce, not just "it still runs."
2. **A new unit test, `tests/unittests/test_get_num_xcc_cache.py`**, following `test_get_rocm_version_cache.py`'s mocked pattern: asserts the call count drops to 1 for repeated calls with the same `device_id`, for the `device_id=None` default-resolution path, and that two different `device_id`s get two separate calls (not wrongly shared). The test file itself needs the full `iris` package import (`triton`/`tritonBLAS`), which isn't installable here without ROCm hardware — the same reason the distributed suite is excluded in `ship-manifest.md` — so I verified its exact assertions against the real `hip.py` on this branch via the isolated-import technique, and will rely on CI to run the committed file directly, same as PR #595 did for its own cache test.
3. **The manifest's lint/format checks**, run on the branch: `pipx run "ruff<0.16" check .` and `pipx run "ruff<0.16" format . --check`, both clean.

**Toolkit**

- Tool: the same isolated-import repro script from unit 6 (repro-check already graded it `accept`), re-run against both `main` and the fix branch inside a fresh Docker container each time, plus `pipx run ruff` for the manifest checks.
- What it produced: a before/after call-count delta (5 → 1) tied directly to the fix, three passing test assertions verified against the real module, and clean lint/format output — all in `test_evidence.md`.
- Difficulties: the committed test file can't actually run via `pytest` here, since `from iris.host.platform import hip` pulls in the top-level `iris/__init__.py` (triton, git-sourced `tritonBLAS`) that needs a ROCm toolchain I don't have — the same hardware gap as the full suite, just one import deeper than I expected going in.
- What changed: instead of running the committed test file directly, I re-used the isolated-import harness (stubbing the parent packages, loading `hip.py` from its file path) to run the identical assertions against the real code on the branch, and documented in `test_evidence.md` that CI is still the source of truth for the committed file actually executing.

---

## Implementation Notes

**2026-11-09**

Installed the ship-gate (unit 7) and wrote `ship-manifest.md`: `pipx run "ruff<0.16" check .` and `pipx run "ruff<0.16" format . --check` as the must-pass checks (matching `.github/workflows/lint.yml`'s pin and command, with `--check` on the formatter so it reports instead of rewriting files into my diff), and the full `tests/run_tests_distributed.py` suite excluded for needing 2 real AMD GPUs. Tested both PreToolUse hooks with a bare `gh pr create --help` from a nested Claude Code session: both the voice guard and the ship-gate stopped it independently, confirmed in `~/.claude/voice-guard/guard-log.md`.

Ran plan-check on `plan.md`/`plan-comment.md` against `ROCm/iris#590`: `accept` on the first pass. Built the fix on branch `jesswsong/cache-get-num-xcc`: split `get_num_xcc(device_id=None)` in `iris/host/platform/hip.py` into a public resolver and a new `@functools.lru_cache`-decorated private `_get_num_xcc_cached(device_id)`, exactly as planned — no deviation from `plan.md`. Added `tests/unittests/test_get_num_xcc_cache.py` with three cases (same device repeated, the `device_id=None` default path, two distinct devices). Lint and format both pass clean on the branch. Verified the new test's assertions and the before/after reproduction delta (5 calls → 1) against the real `hip.py` via the isolated-import harness in Docker, recorded in `test_evidence.md`, since the committed test needs the full `iris` package import chain (`triton`/`tritonBLAS`) that isn't installable without a ROCm toolchain.

On 2026-11-10, a ROCm reviewer tested the PR on MI355X with ROCm 7.2.1. They reported correct current-device switching across 8 GPUs, no caching of an invalid device, `get_num_xcc(0)` improving from 547 ns to 37 ns, `get_num_xcc()` from 853 ns to 332 ns, and `Config()` construction from 1.44 µs to 0.95 µs; the reviewer also reported the new tests and distributed test runs passing on hardware. Their review found a gap in my tests: they did not prove that the no-argument call follows a changing current device. I added a regression test for device sequence 0 → 1 → 0, with distinct XCC counts, and simplified the nearby code comment per their nit. These edits are local pending my review and permission to commit/push.

**Toolkit**

- Tool: Claude Code, building the fix directly from the accepted `plan.md` and running `ruff`/the isolated-import verification harness in Docker as I went.
- What it produced: the `hip.py` diff, the new test file, and `test_evidence.md`'s before/after call-count evidence (5 → 1).
- Difficulties: the committed test file can't run via plain `pytest` locally (needs the full `iris` package import chain, which needs a ROCm toolchain I don't have) — not something the plan anticipated in that much detail, though the manifest already anticipated hardware gaps generally.
- What changed: verified the committed test's logic with the isolated-import harness instead of running it directly, and documented that CI is the actual source of truth for the committed file executing — otherwise, no deviation from `plan.md`.

---

## Pull Request

**PR Link:** [ROCm/iris#638](https://github.com/ROCm/iris/pull/638)

**PR Description:** The PR fixes issue #590 by caching `get_num_xcc()` per resolved device, with a regression test ensuring a no-argument call follows the currently active device. In review, a ROCm contributor validated device switching and invalid-device behavior on MI355X, reported `get_num_xcc(0)` improving from 547 ns to 37 ns, `get_num_xcc()` from 853 ns to 332 ns, and `Config()` construction from 1.44 µs to 0.95 µs, and confirmed the tests passed in pytest and distributed runs. I could not run these hardware checks locally because I do not have AMD GPU access.

**Maintainer Feedback:**

- 2026-11-10: A ROCm contributor reviewed the PR, confirmed the implementation and hardware results, and requested a current-device switching regression test; they also suggested shortening a comment. They noted that `iris/host/tracing/core.py` passing rank as device ID is a separate issue.

**Status:** Iterating — addressing the requested regression test and comment nit.

**Toolkit**

- Tool: ship-gate for manifest and PR prechecks, voice-guard for the outgoing PR description, GitHub CLI to open the PR against `ROCm/iris`, and the maintainer's MI355X review for hardware validation.
- What it produced: a passing ship-gate result, the open PR [ROCm/iris#638](https://github.com/ROCm/iris/pull/638), and concrete device-switching, performance, and test evidence from review.
- Difficulties: Anaconda's `gh` shadowed the GitHub CLI in the gate process. The initial test suite also missed the key default-device-switching case, which the reviewer showed by demonstrating that the existing tests passed even with an incorrect cache on `None`.
- What changed: I prioritized the real CLI in `~/.zshrc` and gate invocations, aligned the PR wording to `Fixes #590` after the hardware checks confirmed the remaining issue items, and added the requested 0 → 1 → 0 regression test plus the comment cleanup. The updates are local; I will not commit or push without permission.

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
