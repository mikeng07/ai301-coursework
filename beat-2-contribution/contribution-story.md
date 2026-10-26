# Contribution: SqrtInit raises AttributeError when called on Keras 2 and Keras 3

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Mike
**GitHub Username:** mikeng07
**Issue:** [SqrtInit raises AttributeError when called on Keras 2 and Keras 3](https://github.com/JesperDramsch/keras-complex/issues/37)
**Pull Request:** [fix: use np.sqrt instead of K.sqrt in SqrtInit](https://github.com/JesperDramsch/keras-complex/pull/43)

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

**Unit 5: built issue-scout and ran it on a real wild search**

Overlaid my Unit 1 `issue-select` skill into `issue-scout` (rubric and evidence guide carried over unchanged) and wrote `search-strategy.md`: three queries tracing to my fit profile's Python, CLI/dev-tooling, and named-exception-bug sentences, plus ranking filters over the free search-result fields. The real search broke twice: `gh search issues` silently returns zero results when a quoted multi-term `"A OR B OR C"` phrase is combined with `--language`/`--state` (worked around by splitting into single-keyword calls), and my first global ranking rule let one query's flood of zero-comment issues drown out the other two queries entirely (fixed by ranking each query group independently and interleaving round-robin). The rubric itself needed no patch: it correctly rejected a real candidate with an already-open PR and a real candidate with an unresolved design question embedded in its own issue body, both on the first pass. Full scan output in `~/.claude/skills/issue-scout/scan-output.md`; friction detail in `scan-notes.md`.

**Unit 6: built voice-guard and wrote this repo's CLAUDE.local.md, caught one real overclaim before it posted**

Turned the voice guide from something I read before writing into something that actually gates a post: `voice-guard` reads `voice-guide.md` (now one canonical file at `~/.claude/skills/voice-guide.md`, with `repro-check/`, `plan-check/`, and `pr-precheck/` holding symlinks instead of three drifted copies) and grades a draft rule-by-rule before it goes out. It caught something real on run 1 of my actual claim comment for `keras-complex#37`: I'd written that the reporter's root cause "checks out" from my own code read, but I hadn't run anything yet — "checks out" implies verification my own rules don't let me claim from a static read alone. Rewrote it to name the action precisely ("I read `init.py` against `bn.py` and the two differ exactly the way you described") and re-ran the guard clean before posting; both runs are logged in `voice-guard/guard-log.md`. Separately, `JesperDramsch/keras-complex` ships no `CONTRIBUTING.md` at all, so `CLAUDE.local.md` is built from the README's "Status" section plus one orientation pass (repo structure, CI test matrix across Keras 2 and 3, the exact lines the bug lives in) instead — an honest "the input the assignment names doesn't exist here, so here's what I used instead," not a gap I papered over. It stayed under 60 lines by keeping only what the next session actually needs to act on, not a transcript of the orientation run itself.

**Unit 7: built ship-gate, caught a dirty diff on run 1, shipped clean on run 2**

Built `ship-gate`, which wraps `pr-precheck` with `ship-manifest.md` — a repo-specific checklist I wrote and sourced line by line to `JesperDramsch/keras-complex`'s own `.github/workflows/test_python.yml` (no `CONTRIBUTING.md` exists there to lean on instead). The gate's first real run, against my first fix commit, never got to `pr-precheck`'s formal grade: while assembling the PR package I found my branch's diff still carried `repro_37.py`, a scratch reproduction script, riding along from an earlier commit — exactly what `pr-precheck`'s "Diff quality" check exists to catch. Fixed it by tagging that commit on my fork (`repro-37-evidence`, so my contribution story's reproduction link stays valid) and rebuilding the branch as one clean commit off the real `main`. Run 2, against the rebuilt branch, passed every manifest row (two excluded with dated reasons — a packaging check and a docs build a one-line numeric fix can't plausibly break; one test-matrix leg explicitly deferred to CI because this machine has no working Python 3.11) and got a clean `accept` from `pr-precheck`, so I opened the PR. Separately, this week's `voice-guard` runs caught one real near-miss before I even asked the guard: drafting the PR description, I'd written "81 passed → 84 passed" for the Keras-3 suite without ever having re-run the full suite after adding the new tests — re-ran it for real before the claim went anywhere. Full gate history: `~/.claude/skills/ship-gate/gate-log.md`; this week's guard runs: `~/.claude/skills/voice-guard/guard-log.md`.

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I re-ran the full `issue-scout` funnel for real this week instead of reusing unit 5's scan: 7 `gh search issues` calls (700 raw results across the three fit-profile query groups), filtered down to 100/300/201 survivors per group, round-robin interleaved into a top 5. The scout accepted three of the five and rejected two, both for reasons that held up under a real second look: `shmindmaster/crewscore#76` looked alive on every ranking-stage field (commits minutes old, a release pushed that same hour) but turned out to be archived — a fact none of the fields the ranking stage reads (`commentsCount`, `updatedAt`, `assignees`) could have shown, so the grading stage's `isArchived` check earned its keep. `GitDakky/twin-me-up#4` read as a clean good-first-issue from its labels but its own body asked for three separate host integrations ("One host per pull request is perfect") — the "Scope fits a newcomer" check's tracker-not-facet clause caught it cleanly.

Of the three accepted candidates, I picked `JesperDramsch/keras-complex#37` over the two `mrvi0/cli-cheatsheet` translation issues that also passed. All three cleared the rubric, but the rubric only answers "is this a good first issue," not "is this the Python bug my fit profile asks for" — that's the fit profile's job, and it's a clean tie-break here: translating a JSON localization file is not Python code, and this week's work needs an actual bug to run `repro-check` against. `#37` is the opposite: a named `AttributeError` in `complexnn.SqrtInit`, a concrete repro script already in the issue body, a root cause the reporter traced to one line (`init.py` L294-297) against its working sibling in `bn.py`, and even a named possible fix — exactly the "named exception escaping where it should be caught, in one function" shape from my fit profile's sandbox-issue sentence. I read both functions myself before claiming and the difference matches what the reporter described. I want to confirm it by actually running it, trace whether the related `dtype`-ignoring bug in `bn.sqrt_init` is in scope or genuinely optional (the issue says optional), and learn what a real maintainer review looks like on a small, well-bounded numerical bug rather than a web-app one.

Claim comment: https://github.com/JesperDramsch/keras-complex/issues/37#issuecomment-6205292159 (posted 2026-10-15, after a `voice-guard` catch — see the Unit 6 toolkit entry below).

The scout itself held up well on a second real run: no new search/ranking friction (all three unit-5 fixes worked unchanged), and the one new case it had to handle correctly — an archived repo that looked fresh on every cheap signal — it handled correctly because the rubric already required a non-archived repo as a precondition, not because I patched anything. Full grading detail for all five candidates: `~/.claude/skills/issue-scout/scan-output.md`; this week's friction notes (one real gap closed by an existing clause, not a new one): `~/.claude/skills/issue-scout/scan-notes.md`.

**Toolkit**

- Tool: issue-scout (my scout: my search strategy, my rubric), run live against all of GitHub
- What it produced: a graded top-5 (3 accept, 2 reject), fully traceable to the three query groups; `JesperDramsch/keras-complex#37` ranked top among the accepted by fit (exact match to the named-exception-bug sentence; the other two accepted candidates were non-code translation tasks)
- Difficulties: none in the search/ranking machinery this run (unit 5's three fixes held). The rubric correctly rejected an archived repo that every cheap ranking-stage field made look alive, and a multi-host tracker dressed as one good-first-issue.
- What changed: nothing patched this week — confirmed by applying the existing rubric and strategy as written and getting confident verdicts, not `unclear`, on both of this run's harder cases.

---

## Understanding the Issue

**Problem Description**

`complexnn.SqrtInit`, a Keras initializer class in this complex-valued-neural-network library, raises `AttributeError: 'int' object has no attribute 'dtype'` the moment it's called — either directly (`complexnn.SqrtInit()((2, 2))`) or as a layer initializer. It fails identically on Keras 3 and on Keras 2 (via TensorFlow 2.15 or `tf-keras`).

**Expected Behavior**

Calling `SqrtInit()(shape)` should return a tensor of the given `shape` and `dtype`, filled with `1/sqrt(2)` — the same value its working sibling function, `complexnn.bn.sqrt_init`, already produces for the layers that use it by default.

**Current Behavior**

`SqrtInit.__call__` (`complexnn/init.py` L294-297) computes `K.constant(1 / K.sqrt(2), shape=shape, dtype=dtype)`, where `K` is `tensorflow.keras.backend`. `K.sqrt` is the Keras backend's tensor sqrt, and its first line reads `.dtype` off its argument — but `2` is a plain Python int, so it raises before anything is computed. I read this against `complexnn/bn.py` L17-19's `sqrt_init`, which computes `(1 / np.sqrt(2)) * K.ones(shape)` instead: NumPy does the division on a plain number, and only the already-numeric result goes through Keras. The two functions look like aliases of each other (the module even aliases `sqrt_init = SqrtInit` at the bottom of `init.py`) but only one of them actually works.

**Affected Components**

Only `complexnn/init.py`'s `SqrtInit` class. Nothing inside the library calls it: `ComplexBN`, `ComplexLayerNorm`, and `ComplexConv(normalize_weight=True)` all default to the *function* `bn.sqrt_init`, not the class, which is why this has shipped broken since 2017 (per the issue) without any existing test catching it — `tests/test_api.py` only checks that `SqrtInit` is exported by name, which I confirmed by grepping `tests/` for every reference to `SqrtInit`.

**Toolkit**

- Tool: repro-check (`scope.md` overlaid this week to point at this issue; grading still pending until the repro report is written)
- What it produced: scope confirmed, issue read against the live GitHub thread (no comments yet, no competing claim)
- Difficulties: none — the issue's own write-up is unusually thorough (it already traces the root cause and names a fix), so "understanding the issue" was mostly verifying the reporter's own analysis against the actual source, not discovering it cold.
- What changed: nothing in the tool; updated `repro-check/scope.md`'s "Where your issue lives" line from the Path Review placeholder to this real issue, the same overlay pattern `issue-scout/scope.md` got in unit 5.

---

## Reproduction Process

**Environment Setup**

macOS, Python 3.13.5. Forked `JesperDramsch/keras-complex` to `mikeng07/keras-complex`, cloned it, added `jesper` (the real upstream) and `origin` (my fork) as separate remotes, branched `fix/37-sqrtinit-attributeerror` from `main`. `python3 -m venv .venv && pip install -e .` pulled in TensorFlow 2.21.0 and Keras 3.15.1 — one of the exact environments the issue lists. The only friction was time, not difficulty: a fresh `pip install -e .` resolving and downloading TensorFlow from an empty venv took close to ten minutes; no errors, just a slow pull.

**Steps to Reproduce**

1. `python3 -c "import complexnn; init = complexnn.SqrtInit(); init((2, 2))"` (saved as `repro_37.py`)
2. Run it inside the venv described above.
3. Observed: `AttributeError: 'int' object has no attribute 'dtype'`, raised from `keras/src/legacy/backend.py`'s `sqrt`, called from `complexnn/init.py` line 297 inside `SqrtInit.__call__`.

**Reproduction Evidence**

- **Commit showing reproduction:** https://github.com/mikeng07/keras-complex/commit/cabb634562d75c6ccc80d7b7497f91f6e9578e56 (branch `fix/37-sqrtinit-attributeerror`)
- **Screenshots/logs:** full traceback is in the commit message linked above; matches the issue's own Keras-3 traceback frame-for-frame.
- **My findings:** reproduces exactly as described, on the first try, with no deviation from the issue's own repro steps, on two of the issue's three named environments (TensorFlow 2.21.0/Keras 3.15.1, and TensorFlow 2.21.0 + tf-keras 2.21.0 with `TF_USE_LEGACY_KERAS=1`). I could not test the third, plain TensorFlow 2.15.1 (Keras 2.15): this machine has no working Python 3.11 (a dangling `/usr/local/bin/python3.11` symlink) and PyPI has no `2.15.1` wheel for the Python versions I do have — disclosed plainly in the repro report rather than silently skipped. I confirmed the reporter's proposed fix works on both tested environments: swapping `K.sqrt` for `np.sqrt` (the same change `bn.py`'s working `sqrt_init` already uses) returns the expected tensor with no error.

Repro report (posted, then edited once — see Toolkit below): https://github.com/JesperDramsch/keras-complex/issues/37#issuecomment-6406852920

**Toolkit**

- Tool: repro-check (live mode, full package), scope.md overlaid to this issue
- What it produced: a graded `reject` on my first posted repro comment — it was missing OS and install method, failing "Environment recorded" and "Steps runnable by a stranger" even though the reproduction itself was real and correct
- Difficulties: I'd posted the comment before running the check against it. The gap was real: a stranger reading only my comment couldn't have told whether `complexnn` needed a source checkout or `pip install keras-complex`.
- What changed: edited the live comment (not a duplicate post) to add the OS, the exact install command per environment, and re-graded: both checks now pass, full package verdict `accept`.

---

## Solution Approach

**Analysis**

`complexnn/init.py`'s `SqrtInit.__call__` (line 297) computes `K.constant(1 / K.sqrt(2), shape=shape, dtype=dtype)`. `K.sqrt` is `tensorflow.keras.backend`'s tensor sqrt; its first line reads `.dtype` off its argument, and `2` is a plain Python int, so it raises `AttributeError` before anything is computed. `complexnn/bn.py`'s `sqrt_init` function (line 17-19) computes the same value correctly with `(1 / np.sqrt(2)) * K.ones(shape)` — NumPy does the division on a plain number, and only the already-numeric result passes through Keras. Nothing inside the library calls `SqrtInit` (everything defaults to the working `bn.sqrt_init` function instead), which is why this shipped broken since 2017 with no test catching it.

**Proposed Solution**

Swap `K.sqrt(2)` for `np.sqrt(2)` in `init.py` line 297 — the exact fix the issue's own reporter (who is also the repo's maintainer) proposed, and the same pattern `bn.py` already uses. Deliberately NOT touching: `bn.sqrt_init`'s separate, pre-existing bug where it ignores its own `dtype` argument — the issue explicitly flags this as "related but separate" and optional, and it's a different function with a different defect.

**Implementation Plan**

1. `complexnn/init.py` line 297: `K.sqrt(2)` → `np.sqrt(2)`.
2. `tests/test_init.py`: add `TestSqrtInit` — call `SqrtInit` directly, use it as `ComplexBN`'s `gamma_diag_initializer` (the layer-initializer path the issue also names), and check the result matches the already-working `bn.sqrt_init`.
3. `README.md`: remove `SqrtInit` from the "not working in this release" list.

No deviations from this plan during the build. One deviation from my *process*, recorded honestly rather than smoothed over: I built and tested the fix before posting the plan comment, not after — the plan comment says so directly rather than pretending the usual order happened.

**Toolkit**

- Tool: plan-check (live mode), scope.md overlaid to this issue (same real-world overlay pattern as `issue-scout` and `repro-check` got)
- What it produced: a graded `accept` on the posted plan comment across all six required checks (grounded diagnosis, bounded scope with an explicit not-in-scope line, executable by a stranger, a decisive test plan, honest about the one untested environment, and engaging the maintainer's own named fix rather than going around it)
- Difficulties: one genuine tension, not a failure — the plan comment itself makes no claim about the untested Keras-2.15 leg (so it asserts nothing false), but that disclosure lives in the adjacent repro comment rather than being restated in the plan comment itself. The rubric's "Honesty about unknowns" check still passed, since nothing in the plan comment is an unhedged claim, but a plan comment that has to stand alone without an adjacent repro comment next to it would need to repeat the disclosure.
- What changed: nothing patched in the rubric; noted the tension above for the next plan comment to watch for.

---

## Testing Strategy

The issue's own trigger, re-run before and after the fix:

```
# Before (main):
$ python3 -c "import complexnn; complexnn.SqrtInit()((2, 2))"
AttributeError: 'int' object has no attribute 'dtype'

# After (fix/37-sqrtinit-attributeerror):
$ python3 -c "import complexnn; print(complexnn.SqrtInit()((2, 2)))"
tf.Tensor([[0.70710677 0.70710677] [0.70710677 0.70710677]], shape=(2, 2), dtype=float32)
```

Plus a new regression test (`TestSqrtInit` in `tests/test_init.py`) covering the direct call, the layer-initializer path (`ComplexBN(gamma_diag_initializer=SqrtInit())`), and parity against the already-working `bn.sqrt_init`. Full suite + new tests: 84 passed, 1 xfailed on TensorFlow 2.21.0/Keras 3.15.1; 83 passed, 2 skipped (pre-existing) on TensorFlow 2.21.0 + tf-keras 2.21.0. `ruff check .` and `ruff format --check --diff .` both clean. The plain-TensorFlow-2.15.1 CI leg is explicitly not covered locally (see Reproduction Process) — resting on that PR's own CI run for it, not claiming coverage I don't have.

**Toolkit**

- Tool: ship-gate (new this week), wrapping pr-precheck and `ship-manifest.md` (at the clone root, not uploaded — sourced line by line to `.github/workflows/test_python.yml` and my own dated judgment calls)
- What it produced: run 1 (against commit `a272a49`) self-caught a real diff-quality problem before `pr-precheck` even graded it formally — a scratch reproduction script (`repro_37.py`) was riding along in the diff. Run 2 (against the rebuilt, clean commit `3ae5288`) passed every manifest row (two rows explicitly excluded with dated reasons; one row explicitly deferred to CI, not silently treated as a pass) and got a clean `accept` from `pr-precheck` on all four checks. Verdict: ship.
- Difficulties: building the manifest meant actually reading `.github/workflows/test_python.yml` line by line rather than guessing what "ready" means for this repo — the repo has no `CONTRIBUTING.md` to lean on, so every row's source is either the CI file or my own dated judgment.
- What changed: the branch itself. I preserved the original reproduction commit under a fork tag (`repro-37-evidence`) so the "commit showing reproduction" link above stays valid, then rebuilt `fix/37-sqrtinit-attributeerror` as one clean commit before opening the PR. Full gate log: `~/.claude/skills/ship-gate/gate-log.md`.

---

## Implementation Notes

**2026-10-25**

Installed two more venvs to cover the issue's other named environments (plain TF 2.15.1, and TF 2.21 + `tf-keras`). The `tf-keras` one worked on Python 3.13; plain TF 2.15.1 would not install on Python 3.12 or 3.13 at all (no matching wheel on PyPI), and the obvious fix — use the Python 3.11 at `/usr/local/bin/python3.11` — turned out to be a dangling symlink to a framework that isn't actually installed on this machine. Decided not to install a new Python version system-wide just for this one CI leg; disclosed the gap instead and relied on CI.

Made the one-line fix (`K.sqrt` → `np.sqrt`) and confirmed it against both working environments. Added `TestSqrtInit` to `tests/test_init.py` per the issue's own suggestion (direct call + layer-initializer use), and removed `SqrtInit` from the README's "not working" list, also per the issue.

While assembling the PR package for `pr-precheck`/`ship-gate`, caught that my working branch's history (claim → reproduction commit → fix commit) had carried a scratch script, `repro_37.py`, into the diff that would go to the maintainer. Fixed by tagging the reproduction commit on my fork (`repro-37-evidence`, keeping the link in this story's Reproduction Process section valid) and rebuilding the branch as a single clean commit off the real upstream's current `main` before opening the PR.

Also caught, via `repro-check`'s formal grade of my already-posted repro comment: the comment never stated my OS or how I installed the package, failing two required checks even though the reproduction itself was correct. Edited the live comment to add both and re-graded clean.

**Toolkit**

- Tool: the fix itself was straightforward hand-editing, not tool-built; the toolkit work this week is `ship-gate` (Testing Strategy section above) and `voice-guard` + `CLAUDE.local.md` (unit-6 entry, still in daily use — see the Unit 7 Toolkit entry below for this week's guard catches)
- What it produced: a working, tested fix and a PR that passed its own gate before going out
- Difficulties: a local-environment gap (no Python 3.11) that no amount of tooling fixes — only disclosure does
- What changed: process, not tooling — I now read the actual diff before handing it to `pr-precheck`, instead of trusting that "the fix is right" means "the diff is clean."

---

## Pull Request

**PR Link:** https://github.com/JesperDramsch/keras-complex/pull/43

**PR Description:** What/Before-after/Scope/Testing/AI-disclosure sections, adapted from the Solution Approach and Testing Strategy sections above: names the exact root cause and fix, shows a real before/after transcript of the issue's own trigger, states the deliberate not-in-scope line, names every test run with its command and outcome (including the one CI leg not run locally), and discloses AI assistance even though the repo requires no disclosure.

**Maintainer Feedback:**

- 2026-10-25: none yet on the PR itself. The `labeler` and `pre-commit.ci` checks passed automatically; the repo's main `Test Python package` workflow (the lint/package/docs/test-matrix job this story's `ship-manifest.md` is sourced from) shows `action_required` — GitHub's standard gate requiring a maintainer to approve workflow runs from a first-time contributor. Not a failure, and not something I can resolve myself; noting it as the normal state it is rather than as a problem.

**Status:** Awaiting review.

**Toolkit**

- Tool: ship-gate (same tool as the Testing Strategy section's entry — its verdict is what cleared this PR to open)
- What it produced: a PR opened only after a real `ship` verdict, with every manifest row either passing, excluded with a dated reason, or explicitly disclosed as resting on CI
- Difficulties: none met opening the PR itself; all the real friction (the diff-quality catch, the environment gap) happened before this step and is recorded above
- What changed: nothing further; this section will get a dated entry whenever CI finishes or the maintainer responds

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
