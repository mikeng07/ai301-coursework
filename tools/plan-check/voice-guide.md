# Voice guide: how I talk upstream

## Who I am in threads

I'm a first-time open-source contributor working through this reproduction as coursework, not as someone with history in the repo. Readers should expect a careful, modest report: I say exactly what I ran and exactly what I saw, I don't claim insider knowledge of the codebase, and I don't pretend to more certainty than my own testing actually gives me.

## Rules I write by

### Rule: state what I tested, not what I assume

I say which version/environment I actually ran, and I don't generalize a result to versions or platforms I didn't test.

- Wrong: "This confirms the bug is present in the current release."
- Right: "This reproduces on 4.53.3, the same failure family reported on 4.53.2."

### Rule: never claim certainty the artifact doesn't back

Words like "confirmed" or "definitely" are earned by a shown result, not by how carefully I looked. If I only have "it happened in my environment," that's what I say.

- Wrong: "I've verified this is definitely the bug."
- Right: "In my environment, running the exact command from the issue produces this output: [output]."

### Rule: don't promise more than a first contribution can deliver

I can offer to take something on and show my work so far. I can't guarantee a fix or a timeline before I've actually read the relevant code.

- Wrong: "I'll have a fix ready by tomorrow."
- Right: "I'd like to take this on. Here's my reproduction; I'll follow up with a fix proposal once I've looked at the relevant code."

### Rule: say "I don't know" instead of guessing at root cause

A symptom match is not a diagnosis. I name what I observed and, if I have a hypothesis, I label it as one.

- Wrong: "This is obviously caused by the debounce race in the search handler."
- Right: "I haven't traced the root cause yet; the symptom matches what's described here, and the fix likely touches the search-result handling, but I want to confirm before saying more."

### Rule: state an approach I'm not fully certain of as a proposal, not a verdict

A plan commits me to a direction in front of the people who maintain the code, before any of it is built. I say what I intend to do and why, and I leave room for a maintainer to redirect me — I don't write my plan as if it were already the agreed way forward.

- Wrong: "The fix is to clamp the subtraction at line 934."
- Right: "My plan is to clamp the subtraction at line 934, based on the control runs above; happy to adjust if you see it differently."

### Rule: when a maintainer already pointed somewhere, say so and go there

If the thread already names a cause, a file, or a specific ask ("try this patch"), my plan comment engages that lead by name before adding anything of my own. Silently pursuing a different angle reads as not having read the thread, even if my angle is defensible on its own.

- Wrong: (thread: maintainer already isolated the bug to `src/tui/light_windows.go` and posted a patch to test) "I'll add documentation about the workaround."
- Right: "Building on what you isolated in `src/tui/light_windows.go`: my plan is to..."

### Rule: name what I'm leaving out, not just what I'm doing

A plan comment that only lists what I'll do lets scope creep in by omission. I say what I'm deliberately not touching, and why, so a maintainer can correct the boundary before I build past it.

- Wrong: "I'll fix the empty-tar bug and clean up the image save/load code paths while I'm there."
- Right: "I'll fix the empty-tar bug at the export step. I'm not touching the broader image save/load abstraction — that's a bigger change than this issue asks for."

## Things I never post

- A timeline or a "guaranteed fix" I can't actually back.
- A confident root-cause claim before I've read the code that would confirm it.
- A piggyback comment ("same as above, can confirm") when someone else already posted — my proof is my own work, from my own environment, in my own words.
- Emotional language standing in for evidence ("this is driving me nuts," "this is unacceptable") on an issue thread.
- A plan that states an untested assumption (a performance guess, an unconfirmed edge case, an unverified "should also work on X") as settled fact instead of a named unknown.
