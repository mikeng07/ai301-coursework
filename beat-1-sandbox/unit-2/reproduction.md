# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

mikeng07

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5785826794

Hi, I'd like to take on this as a first contribution. `verify_password` currently lets `passlib.exc.UnknownHashError` escape when the stored hash isn't a recognizable bcrypt hash, instead of failing closed and returning `False` — I'll reproduce this locally and report back with the details, then follow up with a fix that returns `False` and removes the `xfail` marker on `test_verify_with_wrong_hash_format` (manifest H-05).

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5785839790

**Environment:** macOS 14.7.7 (arm64), Python 3.13.5, repo at commit `f89c06f`. This bug lives entirely in `core/security.py`, and `Settings()` (`core/config.py`) has a default for every field, so no `.env`/Docker stack is needed to trigger it — just a venv with the relevant deps:

```
$ python3 -m venv .venv && source .venv/bin/activate
$ pip install "passlib[bcrypt]>=1.7.4" "bcrypt>=4.0.1,<5.0.0" "python-jose[cryptography]>=3.3.0" "pydantic[email]>=2.5.0" "pydantic-settings>=2.1.0"
```
Installed: passlib 1.7.4, bcrypt 4.3.0.

**Steps and observed:**

Control run (a valid bcrypt hash, to confirm `verify_password` works normally first):
```
$ python3 -c "
from core.security import verify_password, hash_password
h = hash_password('password')
print('control (valid hash):', verify_password('password', h))
"
control (valid hash): True
```

The reported trigger — the exact string the repo's own covering test uses (`wrong_hash = "not_a_valid_bcrypt_hash"` in `tests/unit/test_security.py`):
```
$ python3 -c "
from core.security import verify_password
verify_password('password', 'not_a_valid_bcrypt_hash')
"
Traceback (most recent call last):
  ...
passlib.exc.UnknownHashError: hash could not be identified
```

Also ran the repo's own covering test directly:
```
$ python3 -m pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format -v
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format XFAIL
```
`XFAIL` matches the `@pytest.mark.xfail(strict=True, reason="issue #72 (manifest H-05): ...")` marker already on that test — the bug is present exactly as the marker describes it.

**Expected:** `verify_password` returns `False` for a hash it cannot identify (fail closed), the same way it returns `False` for a merely wrong password.
**Actual:** `passlib.exc.UnknownHashError` propagates out of `verify_password` uncaught, as shown above.

One unrelated note for honesty: passlib 1.7.4 against bcrypt 4.3.0 also prints a harmless `(trapped) error reading bcrypt version` warning on every call (a known passlib/bcrypt version-detection quirk). It shows up in both the control and bug runs and has nothing to do with this issue — flagging it so it isn't mistaken for evidence of anything else.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. `agreement: 20/20 scored items  (bar: 18/20: PASS)` — first full run, first-draft rubric.
2. `agreement: 19/20 scored items  (bar: 18/20: PASS)` — confirming run for `--save-run`; `pkg-05` flipped from accept to reject on "Faithful trigger" purely from grader variance (no rubric edit between runs 1 and 2).
3. `agreement: 20/20 scored items  (bar: 18/20: PASS)` — final run, after revising "Faithful trigger" to separate minimization from an operator/input-class swap. This is the run saved to `eval-run.txt`.

**Package analysis**

`pkg-05` (conda `EnvironmentSectionNotValid` breaking JSON output). Gold label: `accept` ("minimal env.yml repro with a json.tool parse failure as the artifact"). My rubric's run-2 result: `reject`, failing "Faithful trigger" — the check's wording ("compared character-for-character... the same one the issue describes") let the grader read the report's self-authored, minimized `env.yml` (a small file with the same invalid `category:` section, not the issue's exact conda-lock URL) as "not faithful," when it was actually a legitimate minimization of the same triggering condition — the same error message, the same invalid section name. I revised the check to explicitly separate minimizing an input (still faithful) from swapping the operator/input class itself (not faithful — the calib-03 trap, `1: {}` instead of `1 = {}`). Re-graded, `pkg-05` became `accept`, matching gold.

**Check rationale**

> Pass if the trigger exercises the same condition the issue describes — the same operator/mechanism and the same kind of input — even if minimized or self-authored (a smaller file that still contains the same invalid section, a shorter command that still hits the same code path). A deliberate variation from the issue's exact version/environment (e.g. a control run, a version bump) also passes if the report labels it as a variation. Fail if a different operator, argument, or input class produced the artifact (changing what the input actually means, not just its size) and the report presents it as confirming the issue anyway.

This is the check named "Faithful trigger" in `rubric.md`, exactly as it reads now. It reads this way because the original wording only had one axis (matches the issue's trigger, or doesn't) and the pkg-05 disagreement showed there are really two different things a report can do to the issue's input: shrink it while keeping the same underlying condition (fine), or change what the input actually means (not fine, and exactly what the calib-03 operator-swap trap tests for).

**Trade-offs**

Loosening this check risked letting a real wrong-target package slip through disguised as a "minimization," so before spending money on a full re-run I re-checked the canaries that depend on this exact check catching a mismatch: `--only pkg-02,pkg-08,pkg-16` (three scored wrong-target packages) plus `calib-03` (the operator-swap trap this check was originally written for) with `--include-calibration`. All four still correctly rejected — the loosened wording didn't cost any of the cases it was built to catch, only fixed the false negative on pkg-05.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
