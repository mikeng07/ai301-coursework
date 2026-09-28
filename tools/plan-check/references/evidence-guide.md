# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

Where it lives: in an eval bundle, the candidate plan's "Diagnosis" (or equivalent opening) section, read against every line of the "Repro evidence" block — its numbered steps, its artifact/output, and every control run it reports. In live mode: the draft `plan.md`'s stated cause, read against the student's own posted repro comment on the issue (or, on the house issue, the house repro pack as quoted in the drafts).

What good looks like: the stated cause explains every step the repro evidence shows, not just the failing one. A control run is the sharpest signal here — a control that removes the blamed mechanism and still fails, or that keeps it and still succeeds, contradicts the diagnosis outright, however confidently the plan states it (pkg-01's tokenizer claim is ruled out by its own control run at step 2, which parses fine with no flag at all; pkg-11's collect-operator claim is ruled out the same way). A diagnosis that only explains the failing step while ignoring a control that contradicts it is not grounded even if it sounds plausible.

## Scope

Where it lives: in an eval bundle, the candidate plan's scope statement (an explicit in-scope/not-in-scope line, when present) and, regardless of whether one is stated, everything the "Approach," "Proposed changes," or "Files" section actually commits to doing. In live mode: the same sections of the draft `plan.md`.

What good looks like: the described work is one change sized to what the issue and repro evidence call for. The tell is not length or section count — a plan can be long and still bounded (pkg-20's numbered approach) or short and still creep (a one-line "and let's also modernize X while we're there"). Watch for a plan that begins from the right diagnosis and then keeps adding: a second unrelated upgrade, a new abstraction layer nothing in the issue asked for, a CI matrix, a settings panel. Explicit deferral of adjacent work ("not in scope: X, because Y") is what a bounded plan looks like even when X is tempting; doing X anyway is scope creep even if the core fix inside it is correct.

## Executability

Where it lives: in an eval bundle, the candidate plan's "Files," "Approach," or numbered-steps section. In live mode: the same sections of the draft `plan.md`.

What good looks like: named files or areas, and one chosen approach — not a menu of options ("upstream or vendored, whichever is easier"), not an undecided layer ("gocui? tcell? not sure"), not a fix that is itself the thing still to be discovered ("investigate where the time goes... optimize whatever the profiling turns up"). A stranger reading only the plan should be able to open the named file and start. A step that says "check nearby call sites for the same pattern" is fine when it sits inside an already-committed fix; it is not fine when it stands in for the fix itself.

## Test plan

Where it lives: in an eval bundle, the candidate plan's "Test plan" section, read against the "Repro evidence" block's own steps and artifact. In live mode: the same section of the draft `plan.md`, read against the student's posted repro comment (or the house repro pack).

What good looks like: a named, observable outcome tied to the actual fix — re-running the repro evidence's exact steps and stating what result now confirms the fix (an exit code, a specific value, the absence of a specific error), or a new regression test with a stated assertion. "Run the full test suite," "confirm nothing else regressed," or "should feel fast" name no outcome specific to this fix and fail this check even standing next to an otherwise strong plan (calib-04 is the clean case: a genuinely sound, bounded plan held back by exactly this). A full-suite run stated in addition to a named, fix-specific check is fine — it only fails when it is the only thing offered.

## Honesty

Where it lives: in an eval bundle, the candidate plan's stated risks, unknowns, or open questions, read against the rest of the plan's claims for anything asserted without having been checked. In live mode: the same, in the draft `plan.md`, plus its "Deviations" section once a build has happened.

What good looks like: telling apart a hedged unknown ("I have not yet measured the per-print cost... if it shows up in the print benchmark I will move the check," pkg-20's stated risk) from an unverified claim dressed as settled fact (an untested cross-platform assumption, a performance guess presented with no measurement behind it, a root cause claimed beyond what the repro evidence actually established). A plan with nothing left uncertain passes by having nothing to hedge; a plan that has an uncertainty and names it as one also passes. A plan that has an uncertainty and states it flatly anyway is the failure this check exists to catch.

## Comms

Where it lives: in an eval bundle, the candidate plan comment's text, read against the "Thread highlights" block for anything a maintainer explicitly identified or requested, and against the "Repo facts" block's stated contribution policy for an AI-use disclosure requirement. In live mode: the draft plan comment, read against the live issue thread and the repo's stated CONTRIBUTING/AI-policy files per `scope.md`.

What good looks like, in two independent parts:

- Thread direction: when a maintainer already named the exact cause, file, or line (junegunn's "this seems to be the culprit," pointing at `src/tui/light_windows.go`, in pkg-04) or posted something asking for engagement (a patched test binary to try), a ready plan comment engages that lead rather than announcing an unrelated path (a docs-only workaround) in silence. When the thread has no such direction — no comments, or comments that don't name a cause or ask anything — this half passes with nothing to engage.
- AI disclosure: when the repo-facts block states an AI-use disclosure requirement (ghostty's AI_POLICY.md in pkg-20, requiring disclosure of tool and extent of assistance), the comment must actually disclose it, not merely be well-written and on-topic (pkg-20's plan is otherwise excellent and still fails here). When the repo states no such requirement, or a permissive one with no disclosure duty, this half passes automatically. In live mode, remember every package here is AI-assisted work by course design, so this half is never "not applicable" once a repo's policy requires it.
