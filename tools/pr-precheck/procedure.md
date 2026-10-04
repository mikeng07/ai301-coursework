# Procedure: how this tool grades a PR package

## Read order

1. Read the issue first (title, body, expected behavior).
2. Read the thread highlights next. Note verbatim any maintainer-authored diagnosis or explicit direction; if none, note "no maintainer direction."
3. Read the repo-facts block third: the stated PR-template sections/checklist, and the stated contribution/AI-disclosure policy (or its absence).
4. Read the plan-context block fourth, before the candidate PR: the plan's stated scope (named files/areas, not-in-scope line), any deviation note, its repro evidence, and every scenario its test plan names. List the test plan's scenarios explicitly (there may be more than one, per calib-04). This must happen before the diff is read, because "in scope" and "decisive test" are defined by the plan, not discovered from the PR.
5. Read the candidate PR's diff and commit list fifth — before the description — so the description's claims get checked against the diff's actual content, never the reverse. Note every changed file/hunk and whether it maps to something the plan named.
6. Read the candidate PR's description sixth, noting every fidelity claim ("implements the plan exactly," "no other changes," "X now also does Y") to compare against what was recorded in step 5.
7. Read the test-evidence section last, against the scenario list from step 4.

In live mode, insert a step 0: read `scope.md` and confirm the target repo matches; note any house rule that changes a later read. Then also read `voice-guide.md` before grading the title and description in step 6.

## Evidence gathering

- **Plan fidelity**: from steps 4-6, record each diff file/hunk as "in scope," "covered by a stated deviation note," or "undeclared addition." Record each description fidelity claim as "matches the diff" or "contradicted by the diff," quoting the specific line of the diff or its absence.
- **Test evidence**: from step 4's scenario list and step 7, identify which named scenario is the plan's primary/triggering one (the actual issue repro) versus secondary (a control, a warning, a second failure mode). For the primary scenario, record whether a concrete before/after is shown and whether it was produced by the actual named repro/trigger (not an adjacent path). For each secondary scenario, record whether it is addressed by any concrete means — a shown transcript, a specific stated claim, or a named regression test whose assertion covers it — or left entirely unaddressed (no claim, no test, no mention anywhere in the PR). Record separately whether the repo's own checks/suite are named (a specific command or target) with a stated pass outcome, noting the pass count when given but not requiring one.
- **Diff quality**: from step 5, scan every hunk and commit message for debris: commented-out code, dead/unused functions, debug prints, bare TODOs on unrelated lines, pure formatting/import churn, and placeholder commit messages. Record each instance found, quoted.
- **Standards and comms**: from steps 2 and 3, record the template's required sections/files and the disclosure requirement (or their absence); from step 6, record whether each required section has real content, whether a required accompanying file appears in the diff (from step 5's file list), whether disclosure is present when required, and whether the PR's approach matches any maintainer direction noted in step 2.

## Check execution

Execute the four checks in this order: Plan fidelity, Test evidence, Diff quality, Standards and comms. Grade each once from the evidence already gathered above; do not re-read the whole package per check.

Grade each `pass`, `fail`, or `unclear` per `rubric.md`'s pass conditions, with a one-line quote or fact as evidence. When evidence for a check is genuinely absent from the package (e.g. the repo states no template and no AI policy, or the thread has no maintainer comments), Standards and comms defaults to `pass` on the parts with nothing to violate, per the rubric's stated default — this is not `unclear`, since "nothing was asked" is itself a determinate fact. Plan fidelity, Test evidence, and Diff quality never default to pass on a missing section: a PR with no test-evidence section, or a diff the package does not actually include, has failed to supply what that check needs, so grade `fail`, not `unclear`. Reserve `unclear` for when the package's own evidence (the plan's scope line, the repo-facts block, the thread) does not pin a fact down well enough to apply the pass condition — never as a stand-in for a PR that under-delivers.

## Verdict assembly

Apply the rubric's verdict rule: accept only if all four checks pass; any `fail` (including any `unclear`, which counts as `fail`) makes the verdict `reject`. When more than one check fails, quote the first failing check in rubric order (Plan fidelity, then Test evidence, then Diff quality, then Standards and comms) as the deciding evidence in the summary, but report every check's grade in the JSON `checks` array regardless of which one decided the verdict.
