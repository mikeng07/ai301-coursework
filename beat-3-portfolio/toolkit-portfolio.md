# Toolkit Portfolio

Path: `beat-3-portfolio/toolkit-portfolio.md`

This file is Assignment 6, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. It has been waiting here since you created the repo in unit 1;
unit 9 is when you write it.

**Only this page is read.** Your `tools/` folders and your installed skill folders are never
opened, so every piece of evidence you want seen has to live inline here: an output excerpt,
a before/after pair, a dated commit reference.

**Write every sentence yourself.** AI may gather diffs, dates and excerpts for you; it does
not draft entries.

**Keep every section header below under its exact name** — Featured Tools, Inventory, The
Arc, and Limitations and What's Next. Feature three or four tools, each in the entry shape
shown below; give every other tool one inventory line.

**Some lines below are labelled, and the label is how they are read.** Three places on this
page use a `Label: your words` shape — the arc's beats, the tool-limits list, and nothing
else. Keep those labels exactly as written, one per line, and never delete a labelled line:
where you have nothing to put after a label, write `none` and move on. A deleted label reads
as a deleted answer.

**Before you finish**, replace every `[bracketed prompt]` with your own words, then delete
the unused `[Tool name]` entry stub, its copy-me sentence, and the prompt sentences under
each header. A leftover bracketed prompt reads as unfilled.

**Student:** Mike

## Featured Tools

I'm featuring issue-scout, voice-guard, ship-gate, and pr-monitor because each of them
changed because something real went wrong, not just because a unit asked for a new tool.
issue-scout and voice-guard both have dated fixes behind them — a broken search query and
a caught overclaim — and voice-guard caught the same kind of mistake in three separate
weeks. ship-gate and pr-monitor are the two biggest catches of the whole contribution: one
kept a messy diff out of a maintainer's hands before the PR ever opened, the other found
out five weeks later that the PR was never going to win anyway. repro-check, plan-check,
and pr-precheck did real work too, but each mostly worked as designed on the first try, so
they're in the Inventory below instead.

**issue-scout**

**What it is, and the judgment hole I wrote:** issue-scout searches all of GitHub for
first-contribution candidates and grades them against a rubric I wrote in unit 1 — checks
like "Nobody is already on it," "Scope fits a newcomer," and "Maintainer alive" — plus a
fit profile that says what kind of issue actually suits me.

**How it evolved, dated:** On 2026-10-11, the first real scan broke twice in ways that had
nothing to do with the rubric itself. A quoted OR-phrase query (`"cli OR command-line OR
terminal" --language python --state open`) silently returned zero results instead of
erroring, which I only caught because the other queries worked fine alone. I split every
OR-phrase into separate single-keyword calls instead. The same run's ranking stage sorted
every result globally by comment count and date, which let one query's flood of brand-new
zero-comment issues crowd out the other two queries — including the one that actually
matched my fit profile. I fixed it by ranking each query group on its own and interleaving
the top of each. Both fixes went straight into `search-strategy.md` that day. The
2026-10-15 re-run, the one that actually found `keras-complex#37`, held with zero new
friction from either fix.

**Evidence:** `shmindmaster/crewscore#76` looked alive on every field the ranking stage can
see — commits minutes old, a release pushed that same hour — but `gh repo view` showed
`"isArchived": true`. The rubric's own archived-repo precondition caught it cleanly; the
ranking stage's fields alone never could have. A second real candidate,
`GitDakky/twin-me-up#4`, read as a clean good-first-issue from its labels, but its own
issue body said "One host per pull request is perfect" — three separate asks dressed as
one issue — and the "Scope fits a newcomer" check caught that one too.

**voice-guard**

**What it is, and the judgment hole I wrote:** voice-guard reads a draft I'm about to post
and grades it line by line against `voice-guide.md` — my own rules for what counts as
overclaiming, guessing, or promising something I can't back.

**How it evolved, dated:** Before unit 6 it was three copies of the same guide, one per
skill, already starting to drift apart. I consolidated them into one canonical file with
symlinks. The first time I actually ran it, on 2026-10-15, against my own first real claim
comment, it flagged my own sentence: I'd written that a reported root cause "checks out
from a read of the code," but I'd only read the code, not run anything. I rewrote it to
name the actual action I'd taken, and it came back clean. The same tool caught two more
real overclaims in later weeks on completely different drafts: on 2026-10-25 I caught
myself, before even running the guard, about to claim a test count I hadn't re-verified
after adding new tests, so I re-ran the suite for real first; on 2026-11-01, closing PR
#43, the guard flagged me stating another PR's test coverage as something I'd verified
myself, when I'd only read it in that PR's own description.

**Evidence:** Before (flagged): "...the root cause you traced checks out from a read of
the code (`init.py` L294-297 vs. the working sibling in `bn.py` L17-19)..." After (clear):
"I read `init.py` L294-297 against the working sibling in `bn.py` L17-19 and the two
differ exactly the way you described." The same pattern again on 2026-11-01 — before:
"...which covers the same fix (and more thoroughly — scalar/vector/matrix shapes,
float16/32/64, `tf.function`, Dense-layer use)..."; after: "Its description also covers
more ground than mine (...) — I haven't run those myself, but no reason to leave two PRs
open for the same fix either way."

**ship-gate**

**What it is, and the judgment hole I wrote:** ship-gate wraps `pr-precheck` with
`ship-manifest.md`, a checklist I wrote by reading `keras-complex`'s actual CI workflow
file line by line, since the repo has no `CONTRIBUTING.md` to lean on instead.

**How it evolved, dated:** On 2026-10-25, the first run never even reached `pr-precheck`'s
formal grade. While assembling the PR package, I noticed my branch's diff still carried
`repro_37.py`, a scratch script left over from reproduction — exactly what the manifest's
diff-quality row exists to catch. I tagged the old commit on my fork so the reproduction
link in this story would stay valid, then rebuilt the branch as a single clean commit off
the real upstream. The second run that same day passed every manifest row (two explicitly
excluded with dated reasons, one explicitly deferred to CI rather than silently counted as
a pass), and `pr-precheck` graded the cleaned package accept on all four checks.

**Evidence:** Gate log, run 1: "did not wait for `pr-precheck` to fail it formally" — the
manifest's own diff-quality intent working before the tool it wraps even ran formally. Run
2: "Verdict: **accept**... Verdict: ship." Same repo, same manifest, one real cleanup in
between.

**pr-monitor**

**What it is, and the judgment hole I wrote:** pr-monitor reads a real PR's live state —
reviews, CI, silence duration, and, every single run regardless of how long it's been
quiet, whether any other PR closes the same issue — against `playbook.md`, which I wrote
before I had a reason to need it.

**How it evolved, dated:** I built it and used it for real the same day, 2026-11-01,
checking in on PR #43 after six days of silence. The every-run competing-PR check found
`#40`: another contributor's PR for the identical bug, opened 2026-10-12, three days
before I even claimed the issue, already carrying the maintainer's sign-off from
2026-10-13. The playbook's rule that a competing PR with maintainer sign-off isn't a wait
situation fired regardless of my own six-day silence threshold, which is the whole point
of writing that rule in before I needed it — it doesn't wait for silence at all.

**Evidence:** monitor-log.md: "Competing PRs on #37: `JesperDramsch/keras-complex#40`...
opened 2026-10-12T21:43:54Z — 3 days before I claimed issue #37 on 2026-10-15." I disclosed
this on PR #43 and closed it myself rather than leave two PRs open for an outcome already
decided before mine existed.

## Inventory

- repro-check: graded my real repro comment on #37 a first-pass reject (missing OS and
  install method, even though the reproduction itself was correct); I edited the live
  comment and it re-graded accept.
- plan-check: graded my real plan comment accept on the first pass across all six required
  checks; it found one real but non-failing tension — the untested-environment disclosure
  lived in the adjacent repro comment rather than being repeated in the plan comment
  itself.
- pr-precheck: the formal grader ship-gate wraps around; ran for real on the cleaned PR
  package and returned accept on all four checks (Plan fidelity, Test evidence, Diff
  quality, Standards and comms).

## The Arc

**Beat 1**
Date: 2026-10-11
Friction: Running issue-scout's search for real for the first time, a quoted OR-phrase
query silently returned zero results instead of erroring, and a naive global ranking let
one query's flood of zero-comment issues crowd out the other two — including the one that
actually matched my fit profile.
Change: Split every OR-phrase into separate single-keyword calls, and reworked ranking to
score each query group independently before interleaving the results.
Result: The 2026-10-15 re-run that actually found `keras-complex#37` held with zero new
friction from either fix, and the rubric correctly rejected two hard real cases (an
archived-but-fresh-looking repo, a multi-host tracker) on its own.

**Beat 2**
Date: 2026-10-15
Friction: voice-guard's first real run, against my own claim comment, flagged me for
writing that a reported root cause "checks out from a read of the code" — I'd only read
the code, not verified anything.
Change: Rewrote the sentence to name the actual action I'd taken instead of a word implying
more certainty than I had, and re-ran the guard.
Result: Posted a clean claim comment the same day; the same guard caught two more real
overclaims of mine in later weeks (a test count on 2026-10-25, another PR's coverage on
2026-11-01).

**Beat 3**
Date: 2026-10-25
Friction: While assembling the PR package for pr-precheck, I found a leftover scratch
script riding along in my branch's diff.
Change: Tagged the old commit on my fork so this story's reproduction link would stay
valid, then rebuilt the branch as a single clean commit.
Result: The second gate run that day passed every manifest row and pr-precheck graded the
package accept on all four checks; I opened PR #43.

**Beat 4**
Date: 2026-11-01
Friction: Checking in on six days of silence on PR #43, pr-monitor's every-run
competing-PR check found that another contributor's PR for the identical bug had existed
three days before I even claimed the issue, and already had the maintainer's sign-off.
Change: Disclosed the overlap on my own PR and closed it myself instead of leaving two PRs
open for an outcome the maintainer had already decided.
Result: PR #43 closed the same day, marked superseded — not merged, not rejected on its
merits, a race I'd lost before I started it.

## Limitations and What's Next

Across the whole toolkit, the pattern that bit me hardest wasn't a tool breaking — it was a
tool never checking a signal it should have. issue-scout graded `keras-complex#37` accept
without ever actually querying for a linked PR, so it missed one that already existed and
was already approved, three days before I claimed the issue. I didn't find that out until
pr-monitor, built five weeks later for an unrelated reason, happened to check the thing
issue-scout never did. I'm deliberately not going back to patch issue-scout's grading of
that issue — the honest record is that it missed something real, not a quiet rewrite of
history. What I'd build next is the obvious fix: add an actual linked-PR query to
issue-scout's rubric (`closingIssuesReferences` via GraphQL, or `gh pr list --search
"linked:{issue}"`), so the next claim doesn't repeat this. I'd also turn
`ship-manifest.md` into something closer to a reusable template with an explicit "source
these rows to your repo's CI file" step, instead of a one-off I built by hand for this one
repo.

**Tool limits.**

issue-scout: the "Nobody is already on it" check only ever verified assignees and
comments, never actually queried for linked PRs — which is exactly how an already-approved
competing PR got through undetected.
voice-guard: it catches overclaiming in wording, not false facts stated with proper
hedging — it's a style gate, not a fact-checker.
ship-gate: `ship-manifest.md` is hand-sourced to this one repo's CI file; a different repo
means rebuilding the manifest row by row, not reusing it as-is.
pr-monitor: used exactly once, and that one run ended the PR's story in week one — its
silence threshold and nudge etiquette have never actually been tested against a thread
that lived long enough to need them.
