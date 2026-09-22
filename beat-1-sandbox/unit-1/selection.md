# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72

**Verdict output**

```
Scope check — codepath/pathreview-ai301-fa26-s1 confirmed; issue #72 is inside it.

Checks (rubric.md):
- Maintainer alive: pass — last default-branch commit 2026-09-16, 6 days before today
  (2026-09-22), inside the 90-day window; repo not archived.
- Repo is in active use: pass — last push to any branch 2026-09-16, inside 12 months;
  no formal release cadence for this repo.
- Scope fits a newcomer: pass — one concrete target ("fail closed, not raise" on a
  malformed hash), named files (core/security.py, tests/unit/test_security.py), an
  xfail marker (manifest H-05) to remove, and a 1-2 hour estimate; no open decision.
- Nobody is already on it: pass — gh issue view 72: assignees: []; no PRs reference
  #72; 0 thread comments.
- AI-assisted contribution is allowed: pass — docs/CONTRIBUTING.md and
  .github/PULL_REQUEST_TEMPLATE.md state no AI policy at all (silence); no AGENTS.md
  or dedicated AI-policy file exists.

Fit note: Python, one file plus its test file, in a security module with a named
1-2 hour estimate — matches the fit profile (Python, prefers a bounded bug fix over
deep-framework work).

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72",
  "checks": [
    {"name": "Maintainer alive", "grade": "pass", "evidence": "Last default-branch commit 2026-09-16, 6 days before today (2026-09-22), inside the 90-day window; repo not archived"},
    {"name": "Repo is in active use", "grade": "pass", "evidence": "Last push to any branch 2026-09-16, inside 12 months; no formal release cadence for this repo"},
    {"name": "Scope fits a newcomer", "grade": "pass", "evidence": "Issue names one concrete target ('fail closed, not raise' on a malformed hash) with named files and a 1-2 hour estimate; no open decision"},
    {"name": "Nobody is already on it", "grade": "pass", "evidence": "gh issue view 72: assignees: []; no PRs reference #72; 0 thread comments"},
    {"name": "AI-assisted contribution is allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md state no AI policy at all (silence)"}
  ],
  "verdict": "accept"
}
```
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. `agreement: 15/20 scored items  (bar: 18/20: below the bar)` — first full run, first-draft rubric.
2. `agreement: 14/20 scored items  (bar: 18/20: below the bar)` — re-run of the same, unrevised rubric; the 15→14 shift was grader variance, not a change I made (`issue-20` flipped accept↔reject with no edit in between).
3. `agreement: 20/20 scored items  (bar: 18/20: PASS)` — final run, after revising the "Scope fits a newcomer" and "Nobody is already on it" checks. This is the run saved to `eval-run.txt`.

**Issue analysis**

`issue-09` (conda `config --clear` option). Gold label: `accept` ("old but valid bounded feature; the 2022 claim is stale and the maintainer invited takers"). My rubric's first-draft result: `reject`, failing "Nobody is already on it" — the check had no staleness threshold, so a 2022 comment ("I'd like to take a swing at this... Does it need to be assigned to me?") registered as a live claim forever, even though no PR was ever opened and the thread had been silent since a bot-driven "(bump)" in 2023 (three-plus years before the 2026 capture date). I revised the check to require *current* follow-through — an open linked PR, or activity within the last 90 days — rather than any claim comment ever posted. Re-graded, `issue-09` became `accept`, matching gold.

**Check rationale**

> Pass if there is no assignee, no open linked PR, and no claim comment that shows current follow-through. A claim blocks the issue only while it shows follow-through: an open linked PR, or activity (the claim itself, a maintainer reply to it, or related commits) within the last 90 days (of the capture date in eval mode, of today in live mode). A claim with no linked PR and no activity in that window is stale and does not block a newcomer, even if a stale-bot once nudged it or a maintainer once said "go ahead" long ago. An open linked PR blocks regardless of its age (hours-old counts).

This is the check named "Nobody is already on it" in `rubric.md`, exactly as it reads now. It reads this way because the first draft treated any claim comment, however old and unfollowed, as a permanent block — `issue-09` showed that a rubric this literal punishes a good, unclaimed issue for a claim that everyone involved had long since abandoned.

**Trade-offs**

The 90-day window is a real trade-off, not a free fix: it means a genuine claim abandoned 91 days ago, by someone who fully intends to come back, would no longer block a newcomer under this rubric — the check can't distinguish "gone for good" from "on a longer break." To confirm the loosened wording didn't let a *real* claim through, I re-ran the canary `issue-13` (an hours-old claim with two already-open linked PRs) with `--only issue-13`: it still correctly rejected, because an open linked PR blocks regardless of age under this check's wording — the staleness window only ever applies to a bare claim comment with no PR behind it.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. Fit to interests and time available: I wanted my first issue to stay in Python (my strongest language) so I could focus on learning the contribution workflow itself, not a new language at the same time. This one is one file plus its test file, with a maintainer-style 1-2 hour estimate attached — realistic for the time I have this week.
2. What the verdict identified vs. what I weighed myself: the rubric mechanically confirmed the things a checklist is good at — the repo is alive and used, the issue is bounded, nobody else has claimed it, and there's no AI-policy wall. What I weighed that the rubric doesn't grade: whether I'd actually enjoy and understand the fix. This one is in a security/auth module handling a known library exception (`passlib.exc.UnknownHashError`), which felt like a concrete, learnable problem rather than an open-ended one.
3. Anticipated difficulty claiming it: low. Zero assignees, zero comments, no linked PRs — the rubric's own accept signal on "Nobody is already on it" is a direct read of exactly this, and it held true when I actually went to claim it.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
