# Evidence guide: where proof lives in a reproduction package

## Environment

Where it lives: in an eval bundle, the repro report's "Environment" line or paragraph (tool version, OS, install method), read against the `Repo facts` block's latest-release line and the issue's own stated environment. In live mode, the same fields in the student's draft repro comment, read against the repo's release page and the issue's environment section.

What good looks like: the tool/runtime version, the OS, and how it was installed or built are all stated, not just one of them. If the issue's behavior is known to change with build profile (debug vs. release — e.g. an overflow that panics in debug and silently wraps in release), the report also names which profile it used; a report that reproduces the artifact but never says debug-or-release on such an issue has not recorded the environment that matters. A version delta from the issue's is fine as long as it is stated out loud, not left for the reader to notice.

## Steps

Where it lives: in an eval bundle, the repro report's "Steps"/"Preparation"/"Execution" section — the literal commands and inputs. In live mode, the same section of the draft repro comment.

What good looks like: every command, file, and config the steps depend on is given in the report itself, so a stranger with no other access could run the same sequence from a cold start. A report that references a private repo, an unshared internal config, or "our staging setup" without including the relevant contents is not followable, however precise its commands look; the reader cannot place the attempt.

## Behavior shown

Where it lives: in an eval bundle, the repro report's shown output/log/error text (the actual artifact), and separately, the exact input/command that produced it — read against the issue's stated trigger (its command line or input file, character for character) and its stated failure (error text, crash signature, UX symptom). In live mode, the same artifact in the draft repro comment, read against the live issue's body and any maintainer trigger notes in the thread.

What good looks like: the trigger exercises the same condition the issue names, not a nearby one. Two things get confused here and must be told apart: minimizing (a smaller or self-authored input that still contains the same invalid section, the same offending value, the same code path) is faithful and expected in a good report; swapping the operator, delimiter, or argument itself (a colon for an equals sign, a prefix range for an offset-from-end range) changes what the input means and produces a different, often more graceful, failure — that swap is exactly the kind of thing a confident, well-formatted report can paper over. Once the trigger is confirmed faithful, the artifact must show the same class of failure the issue describes — same error signature, same crash, same missing behavior — not an adjacent one.

## Honesty

Where it lives: in an eval bundle, the report's stated conclusion (an Expected-vs-Actual line, or a claim of "confirmed"/"cannot reproduce") and the claim comment's certainty language, read against what the Behavior-shown artifact actually demonstrates. In live mode, the same comparison between the draft's stated conclusion and its own artifact.

What good looks like: the conclusion never claims more than the shown artifact supports. An honest, evidenced cannot-reproduce (a real attempt, a shown result, a named difference — environment, version, missing precondition) is a pass; it is proof of an honest negative, not proof of nothing. What fails this: narrating a different artifact as if it confirmed the issue, testing an old or unacknowledged version and treating the result as evidence about the current release, or generalizing a result ("this affects the Store release too") beyond what was actually run.

## Comms

Where it lives: in an eval bundle, the candidate claim comment's text, read against the issue itself (does it name something real about it?) and the repo-facts block's contribution policy, including any stated AI-use disclosure requirement. In live mode, the same reading against the live repo's CONTRIBUTING file and issue templates.

What good looks like: the claim comment names something specific to this issue — the actual symptom, the code area, or a concrete plan — rather than interchangeable enthusiasm ("+1," "this drives me nuts"), a bare self-assignment, or an unconditional promise with nothing behind it. Separately, when the repo's stated policy requires disclosing AI assistance, the comments say so; when the repo states no such requirement (silent, or "permissive with responsibility"), no disclosure is owed and this family passes without one.
