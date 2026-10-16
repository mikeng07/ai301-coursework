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

**Unit 5: built issue-scout and ran it on a real wild search**

Overlaid my Unit 1 `issue-select` skill into `issue-scout` (rubric and evidence guide carried over unchanged) and wrote `search-strategy.md`: three queries tracing to my fit profile's Python, CLI/dev-tooling, and named-exception-bug sentences, plus ranking filters over the free search-result fields. The real search broke twice: `gh search issues` silently returns zero results when a quoted multi-term `"A OR B OR C"` phrase is combined with `--language`/`--state` (worked around by splitting into single-keyword calls), and my first global ranking rule let one query's flood of zero-comment issues drown out the other two queries entirely (fixed by ranking each query group independently and interleaving round-robin). The rubric itself needed no patch: it correctly rejected a real candidate with an already-open PR and a real candidate with an unresolved design question embedded in its own issue body, both on the first pass. Full scan output in `~/.claude/skills/issue-scout/scan-output.md`; friction detail in `scan-notes.md`.

**Unit 6: built voice-guard and wrote this repo's CLAUDE.local.md, caught one real overclaim before it posted**

Turned the voice guide from something I read before writing into something that actually gates a post: `voice-guard` reads `voice-guide.md` (now one canonical file at `~/.claude/skills/voice-guide.md`, with `repro-check/`, `plan-check/`, and `pr-precheck/` holding symlinks instead of three drifted copies) and grades a draft rule-by-rule before it goes out. It caught something real on run 1 of my actual claim comment for `keras-complex#37`: I'd written that the reporter's root cause "checks out" from my own code read, but I hadn't run anything yet — "checks out" implies verification my own rules don't let me claim from a static read alone. Rewrote it to name the action precisely ("I read `init.py` against `bn.py` and the two differ exactly the way you described") and re-ran the guard clean before posting; both runs are logged in `voice-guard/guard-log.md`. Separately, `JesperDramsch/keras-complex` ships no `CONTRIBUTING.md` at all, so `CLAUDE.local.md` is built from the README's "Status" section plus one orientation pass (repo structure, CI test matrix across Keras 2 and 3, the exact lines the bug lives in) instead — an honest "the input the assignment names doesn't exist here, so here's what I used instead," not a gap I papered over. It stayed under 60 lines by keeping only what the next session actually needs to act on, not a transcript of the orientation run itself.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

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
- **My findings:** reproduces exactly as described, on the first try, with no deviation from the issue's own repro steps. I additionally confirmed the reporter's proposed fix actually works: `K.constant(1 / np.sqrt(2), shape=(2,2), dtype='float32')` (swapping `K.sqrt` for `np.sqrt`, the same change `bn.py`'s working `sqrt_init` already uses) returns the expected `[[0.707, 0.707], [0.707, 0.707]]` tensor with no error. I have not yet checked the Keras-2 (TensorFlow 2.15 / `tf-keras`) environments the issue also lists — that's still open before I write the repro comment and move to the fix.

**Toolkit**

- Tool: repro-check (live mode, full package not yet graded — still only a claim comment plus this reproduction, no repro report posted upstream yet)
- What it produced: a real, matching reproduction on one of the issue's three named environments, committed to my fork for a durable link
- Difficulties: none in the reproduction itself; the only friction was the ~10-minute TensorFlow install, unrelated to the bug
- What changed: nothing in the tool. Still need to reproduce on the Keras-2 environments before the repro report is complete.

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
