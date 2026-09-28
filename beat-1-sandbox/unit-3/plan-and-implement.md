# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

mikeng07

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5861401787

My plan, building on the reproduction above: `verify_password` never catches what `passlib` raises when it can't identify the stored hash's scheme (`passlib.exc.UnknownHashError`), so the exception escapes instead of the function failing closed. I'll wrap the existing `pwd_context.verify(...)` call in a `try/except UnknownHashError: return False`, then remove the `@pytest.mark.xfail` marker on `test_verify_with_wrong_hash_format` (manifest H-05) since the assertion it already makes will hold for real.

I'm keeping this to that one call site — not touching `hash_password`, the JWT functions, or broadening the catch to other passlib exception types, since neither the issue nor my repro produced anything beyond the unknown-scheme case. If a different malformed-hash exception type turns up while I'm building this, I'll flag it as a separate finding rather than widening the fix to cover it speculatively.

Test plan: re-run `test_verify_with_wrong_hash_format` (expect `PASSED`, no longer `XFAIL`), re-run the exact trigger from my repro (expect `False`, not a traceback), and re-confirm the valid-hash control still returns `True` so the fix doesn't touch the working path.

---

## Your branch

**Branch**

fix/72-verify-password-fail-closed

**Evidence**

Re-running my Unit 2 reproduction steps against the built change, before and after:

**Before** (on `main`, commit `f89c06f`):

```
$ python3 -m pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format -v
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format XFAIL [100%]
================= 24 deselected, 1 xfailed, 1 warning in 1.35s =================

$ python3 -c "
from core.security import verify_password
verify_password('password', 'not_a_valid_bcrypt_hash')
"
Traceback (most recent call last):
  File "<string>", line 3, in <module>
    verify_password('password', 'not_a_valid_bcrypt_hash')
  File ".../core/security.py", line 37, in verify_password
    return bool(pwd_context.verify(plain_password, hashed_password))
  File ".../passlib/context.py", line 2343, in verify
    record = self._get_or_identify_record(hash, scheme, category)
  File ".../passlib/context.py", line 1132, in identify_record
    raise exc.UnknownHashError("hash could not be identified")
passlib.exc.UnknownHashError: hash could not be identified

$ python3 -c "
from core.security import verify_password, hash_password
h = hash_password('password')
print('control (valid hash):', verify_password('password', h))
"
control (valid hash): True
```

**After** (on `fix/72-verify-password-fail-closed`, commit `893f9f8`):

```
$ python3 -m pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format -v
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format PASSED [100%]
================= 1 passed, 24 deselected, 1 warning in 0.24s ==================

$ python3 -c "
from core.security import verify_password
print(verify_password('password', 'not_a_valid_bcrypt_hash'))
"
False

$ python3 -c "
from core.security import verify_password, hash_password
h = hash_password('password')
print('control (valid hash):', verify_password('password', h))
"
control (valid hash): True

$ python3 -m pytest tests/unit/test_security.py -v
======================== 25 passed, 1 warning in 5.82s =========================
```

`make lint` passes on the branch. `make typecheck` fails, but on a pre-existing
`numpy` stub/mypy-version incompatibility in `numpy/__init__.pyi`, unrelated to this
change; running `mypy core/security.py` directly reports "Success: no issues found,"
confirming the changed file itself is clean.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke run, `--limit 3` (not scored, sanity check only): 3/3 agreement, first-draft rubric.
2. `agreement: 19/20 scored items  (bar: 18/20: PASS)` — first full run, first-draft rubric/evidence-guide/procedure. Categories: `clear-accept 6/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`. Only disagreement: `pkg-14`, failed check "Executable by a stranger."
3. `agreement: 19/20 scored items  (bar: 18/20: PASS)` — confirming run for `--save-run`, no files edited between runs 2 and 3. Same categories, same single disagreement (`pkg-14`), but the failed-check note shifted to "Executability, Honesty about unknowns" — grader variance on the same package and the same rubric, not a rubric change. This is the run saved to `eval-run.txt`.

**Package analysis**

`pkg-14` (zellij-org/zellij#5174, OSC color leak on SSH reattach). Gold label: `accept`
(category `clear-accept`, note: "honestly scoped-down: reattach handshake fix with a
regression-window repro; defers the untestable Windows variant and says so; arguable
on the deferral, ready as scoped"). My rubric's result: `reject`, failing "Executable by
a stranger" — the plan names the fix site as "the client attach/reattach path in
`zellij-server`... and `zellij-client`'s terminal query issuance," then adds "exact
functions to be pinned in the PR after tracing the query issuance with debug logs,
which I have working." My check's pass condition reads "fail if any decision that
matters to starting the work is left open at plan time... a fix that is itself the
thing still being discovered," and the grader read "exact functions to be pinned in
the PR" as exactly that kind of open decision. Gold reads it the other way: the fix
site and mechanism (a query-response race in the reattach handshake) are already
pinned and already traced with working debug output, so naming the precise function is
a detail, not a discovery — the plan is scoped and ready as-is. Both readings are
defensible on the same sentence, which is why the gold notes call this package
"arguable" too.

**Check rationale**

> Pass if the plan names concrete files or areas and a single chosen approach, in
> enough detail that someone who has never seen the issue could start working without
> asking the author anything. Fail if any decision that matters to starting the work is
> left open at plan time — an unnamed file, an unchosen approach among options,
> "investigate X to find the fix," "upstream or vendored, whichever is easier."
> Exploration steps are fine only when they sit inside an already-bounded,
> already-chosen fix (e.g. "grep for the same pattern at nearby call sites"), not when
> the fix itself is what's still being discovered.

This is the check named "Executable by a stranger" in `rubric.md`, exactly as it reads
now. I wrote it this strict on purpose: `pkg-10` ("investigate... optimize whatever the
profiling turns up"), `pkg-17` ("gocui? tcell? not sure," no files), and `pkg-18`
("recover() somewhere," "upstream or vendored, whichever is easier") are the three
`unbuildable` packages this check has to catch, and all three defer their *central* fix
decision the same way — no chosen mechanism, no chosen files. I rejected a softer
version that would exempt "the exact function name" from counting as an open decision,
because that softening is very close to the wording `pkg-10`'s "optimize whatever the
profiling turns up" or `pkg-18`'s "upstream or vendored, whichever is easier" could also
claim shelter under, and I could not write the exemption narrowly enough to admit
`pkg-14` without also opening a gap for one of those three.

**Trade-offs**

I accept that this check will misclassify a plan that has already pinned the fix site
and mechanism precisely (including a working debug trace, in `pkg-14`'s case) but
defers only the exact function name to the PR — that is a real plan the rubric holds
back, not a bad one. I chose not to loosen the check to admit it, because the three
`unbuildable` packages this check exists for (`pkg-10`, `pkg-17`, `pkg-18`) all defer
their central mechanism, not a subordinate naming detail, and I could not find wording
that reliably tells "mechanism already found, function name still TBD" apart from
"mechanism itself still TBD" without risking a canary flip on one of those three. Since
the category floor for `unbuildable` was already at the minimum non-zero count I'd
want (3/3, all agreeing), I judged the safer trade to be losing `pkg-14` rather than
spending a loosening pass that could cost a package in the family this check was built
to hold the line on.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
