# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/85

**Branch**

fix/72-verify-password-fail-closed

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke run, `--limit 4` (not scored, sanity check only): 4/4 agreement, first-draft tool, one package from each category.
2. `agreement: 16/20 scored items  (bar: 18/20: below the bar)` — first full run, first-draft tool. Categories: `clear-accept 3/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`. Four clear-accept packages (`pkg-05`, `pkg-08`, `pkg-13`, `pkg-16`) wrongly failed "Test evidence."
3. Targeted re-run, `--only pkg-05,pkg-08,pkg-13,pkg-16,pkg-02,pkg-11,pkg-19,pkg-04,pkg-07,pkg-10,pkg-14,calib-04 --include-calibration` after revising the Test evidence check: 11/11 agreement (not a full/bar run) — the four wrongly-rejected packages now passed, and every already-agreeing clear-accept and not-tested canary (plus the `calib-04` trap) still agreed.
4. `agreement: 20/20 scored items  (bar: 18/20: PASS)` — confirming full run, categories `clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`. This is the run saved to `eval-run.txt`.

**Package analysis**

`pkg-09` (sharkdp/fd#2053, "surface the inner error in `--changed-` date parsing failures"). Gold label: `reject` (category `silent-drift`, note: "inner-error surfacing implemented, but the diff also rewords unrelated flag help text and adds a new `--time-format` flag the plan explicitly scopes out, under a description claiming 'exactly as planned'"). My tool's verdict: `reject`, failing "Plan fidelity." The plan's scope line reads "Not in scope: new flags, other error messages, or accepted date formats," and the diff adds exactly that: a new `--time-format` option, plus reworded help text on two unrelated flags and a trim change in `error.rs`'s generic error printer — none of it named anywhere in `plan.md`, and no deviation note covers it. The description compounds it by claiming "Error messages for date parsing improved exactly as planned," which is true of the inner-error hunk alone but false of the diff as a whole once the unannounced flag and reworded help text are counted. My rubric reads this as drift rather than a comms problem specifically because the check's evidence pairing is "every changed hunk against the plan's named scope, and the description's fidelity claim against the diff" — the extra hunks fail the first half regardless of how good the core fix is, which is the same reasoning gold gives ("arguable because the extra work is adjacent, but the new flag is unannounced scope").

**Check rationale**

> Pass if every changed file or hunk falls inside the plan's stated scope, or is covered by an explicit deviation note (in `plan.md` or restated in the description) explaining what changed and why, AND the description's fidelity claims match the diff exactly — it claims neither more than the diff delivers nor less. Fail if the diff touches a file, adds a flag/option, or changes behavior the plan did not scope in and no deviation note covers it (an "adjacent" or "while I'm in there" addition counts, however small), or if the description asserts something the diff contradicts in either direction (claiming full fidelity over an untouched file the diff silently changes; claiming a capability — e.g. "docs updated" — that the diff does not contain). An honestly stated, explicitly scoped-out deferral that both the plan and the description name is not drift and passes.

This is "Plan fidelity" in `rubric.md`, exactly as it reads now. I wrote the "however small" clause on purpose after reading the five `silent-drift` packages together: `pkg-03`'s `truncate()` rewrite and new config option, `pkg-06`'s renaming pass and new settings flag, `pkg-09`'s new `--time-format` flag, and `calib-03`'s silent rewrite of an untouched file are all different sizes of the same mistake — an addition the plan never scoped, however adjacent or small, riding along with a correct core fix. I rejected a softer version that would only flag additions above some size or prominence threshold, because `pkg-09`'s extra flag is genuinely small next to the real fix, and a size threshold is exactly what would have let it through.

**Trade-offs**

My first-draft "Test evidence" check required a shown before/after transcript for *every* scenario the plan's test plan named, with no exception. That wording cost four genuine clear-accepts on the first full run (`pkg-05`, `pkg-08`, `pkg-13`, `pkg-16`): each has a real transcript for its primary repro, but covers a secondary scenario (a same-key warning, a control case) only through a named regression test and a stated suite-pass count, not a second manual transcript. I revised the check to accept a named regression test or a specific concrete claim for secondary scenarios, while still requiring a real shown transcript for the primary trigger. Before spending the confirming full run, I re-ran the four changed packages plus canaries from both ends with `--only`: the three already-agreeing clear-accepts (`pkg-02`, `pkg-11`, `pkg-19`), all four `not-tested` rejects (`pkg-04`, `pkg-07`, `pkg-10`, `pkg-14`), and the `calib-04` trap (whose rejection depends on a *named* scenario going completely unaddressed, not under-evidenced). All eleven still agreed, confirming the loosened wording gained the four real accepts without opening a gap for the vaguer evidence the `not-tested` category exists to catch.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
