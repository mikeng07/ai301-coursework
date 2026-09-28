# Procedure: how this skill grades a plan package

## Read order

1. Read the issue context first (title, body, expected behavior). Note what the reporter says is wrong and what "fixed" would look like.
2. Read the thread highlights next. Note verbatim any comment where a maintainer names a cause, a file/line, or a specific request (e.g. "test this patch"); if there is none, note "no maintainer direction."
3. Read the repro-evidence block third, before the plan. Note, as a short list: every step, every control run and its result, and the stated Expected/Actual. This list is what the Diagnosis and Test-plan checks compare the plan against, so it must be pinned down before the plan is read, not reconstructed from memory afterward.
4. Read the repo-facts block fourth. Note the stated AI-use disclosure requirement, if any (silent/permissive counts as none).
5. Read the candidate plan fifth, end to end, noting which section answers which rubric family: stated cause (Diagnosis), in-scope/not-in-scope line and the full approach/files section (Scope, Executability), test plan (Test plan), and any stated risks/unknowns/deviations (Honesty).
6. Read the candidate plan comment last, since it is graded only against what steps 2 and 4 already produced (thread direction, disclosure requirement).

In live mode, insert a step 0: read `scope.md` and confirm the issue is in the scoped repo; note any house rule that changes how a later step reads (e.g. a classmate's plan on the same issue does not change grading). Then also read `voice-guide.md` before grading the plan comment in step 6.

## Evidence gathering

- **Diagnosis**: from step 3's step/control list, check each control's result against the plan's stated cause from step 5. Record which specific control (if any) is consistent or inconsistent, quoting it.
- **Scope**: from step 5, record the plan's stated not-in-scope line (or its absence) plus every concrete action the approach/files/proposed-changes section commits to. Record each action as either "addresses the issue's own behavior" or "goes beyond it" before judging.
- **Executability**: from the same approach/files section gathered above, record whether a single approach and named files/areas are present, or whether any decision is left open (quote the open decision if so).
- **Test plan**: from step 5's test-plan section, record whether it names the repro evidence's own steps or a specific regression assertion, or only a generic check.
- **Honesty**: from step 5's risks/unknowns section (or its absence) plus a re-scan of the plan's other claims, record any claim that reads as asserted-but-unverified against what step 3 actually established.
- **Comms**: from step 2's thread note and step 4's disclosure note, read the plan comment (step 6) against both. Record whether it engages any noted maintainer direction, and whether it discloses AI use if step 4 found a requirement.

## Check execution

Execute the six checks in this order: Diagnosis, Scope, Executability, Test plan, Honesty, Comms. Each check is graded once, using only the evidence gathered for it above; do not re-read the whole package per check.

For each check, grade `pass`, `fail`, or `unclear`, with a one-line quote or fact as evidence, per the pass conditions in `rubric.md` and the "what good looks like" guidance in `references/evidence-guide.md`.

When evidence for a check is genuinely absent from the package (e.g. no stated risks anywhere, no thread comments at all), that absence has a specific default per check: Diagnosis and Comms default to `pass` when there is nothing to contradict or engage (no controls to conflict with a cause; no thread direction and no disclosure requirement to violate). Honesty defaults to `pass` when the plan has no claims that need hedging. Scope, Executability, and Test plan never default to pass on absence: a plan that states no scope boundary, no files, or no test plan has failed to provide the evidence those checks need, so grade `fail` there (not `unclear`), since the gap is the plan's own, not a limit of what the package could show.

A check may be graded `unclear` only when the package's own evidence (the repro-evidence block, the thread, the repo-facts block) does not pin the fact down well enough to apply the pass condition — never as a stand-in for a plan that under-delivers, which is `fail`.

## Verdict assembly

Apply the rubric's verdict rule: accept only if all six checks pass; any `fail` (including any `unclear`, which counts as `fail`) makes the verdict `reject`. Quote, for the check the summary calls out, the exact fact or line recorded during evidence gathering for that check — never a paraphrase. When more than one check fails, name all of them in the summary, but the JSON `checks` array always reports the grade for every check, not only the failing ones.
