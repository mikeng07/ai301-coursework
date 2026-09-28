# Plan: fail closed on an unrecognized password hash (issue #72)

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72
Manifest: H-05 — "`core/security.py` lets passlib's `UnknownHashError` escape when the
stored hash is not a recognizable format. Verification against a malformed hash should
fail closed (return `False`), not raise. The covering test is marked
`@pytest.mark.xfail` referencing manifest id H-05 — remove the marker as part of the fix."

## Diagnosis

`verify_password` in `core/security.py` calls `pwd_context.verify(plain_password,
hashed_password)` directly and returns its bool result, with nothing catching what
`CryptContext.verify` raises when it cannot identify the hash's scheme:
`passlib.exc.UnknownHashError`.

Grounded in my Unit 2 reproduction (posted at
https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5785839790):
a control run with a real bcrypt hash returns `True` normally (so `verify_password`
itself is not broken generally), and the exact trigger string the repo's own covering
test uses (`"not_a_valid_bcrypt_hash"`) raises `passlib.exc.UnknownHashError: hash could
not be identified` with the traceback rooted at the `pwd_context.verify(...)` call
inside `verify_password`. The repo's own `test_verify_with_wrong_hash_format`, run
directly, reports `XFAIL` matching the `@pytest.mark.xfail(strict=True, reason="issue
#72 (manifest H-05): ...")` marker already on it. All three signals point at the same
single call site with no unhandled input; there is no evidence of a second cause.

## Scope

One bounded change: `verify_password` fails closed (returns `False`) when passlib
cannot identify the stored hash's scheme, instead of letting the exception escape.

In scope:
- `core/security.py`: catch `passlib.exc.UnknownHashError` around the
  `pwd_context.verify(...)` call in `verify_password` and return `False`.
- `tests/unit/test_security.py`: remove the `@pytest.mark.xfail(...)` marker on
  `test_verify_with_wrong_hash_format` (per manifest H-05 and the PR template's
  checklist item for a fixed seeded bug).

Not in scope:
- `hash_password`, `create_access_token`, `decode_access_token`, or any other function
  in `core/security.py` — the issue and the repro evidence are specific to
  `verify_password` against a malformed hash; nothing else in the module is implicated.
- Broadening the except clause to other passlib exception types (e.g. a hash that
  matches a known scheme's format but is internally corrupt, which passlib can raise
  differently for). The issue and the covering test name `UnknownHashError` specifically
  ("hash could not be identified"); I checked `pyproject.toml`'s mypy override list for
  any other H-05-tagged suppression to clear alongside this and found none scoped to
  `core.security`, so the xfail marker is the only other artifact this bug left behind.
- Any change to `pyproject.toml` (no lint/type suppression is tied to this bug).

## Files

- `core/security.py` — the `verify_password` function.
- `tests/unit/test_security.py` — the `test_verify_with_wrong_hash_format` xfail marker.

## Approach

1. In `core/security.py`, import `UnknownHashError` from `passlib.exc`.
2. Wrap the existing `pwd_context.verify(plain_password, hashed_password)` call in
   `verify_password` in a `try/except UnknownHashError: return False`, keeping the
   existing `bool(...)` return for the success path unchanged.
3. In `tests/unit/test_security.py`, remove the
   `@pytest.mark.xfail(strict=True, reason="issue #72 (manifest H-05): ...")` decorator
   directly above `test_verify_with_wrong_hash_format`. Leave the test body as written
   (it already asserts `verify_password("password", wrong_hash) is False`).

## Test plan

Re-running my Unit 2 repro steps against the built change, before and after:

- `python3 -m pytest tests/unit/test_security.py -k test_verify_with_wrong_hash_format -v`
  — before: `XFAIL`. After: `PASSED` (marker removed, assertion now holds for real).
- `python3 -c "from core.security import verify_password; print(verify_password('password', 'not_a_valid_bcrypt_hash'))"`
  — before: raises `passlib.exc.UnknownHashError`. After: prints `False`.
- Control, no-regression check: `python3 -c "from core.security import verify_password, hash_password; h = hash_password('password'); print(verify_password('password', h))"`
  — before and after: `True` (the fix must not touch the valid-hash path).
- `python3 -m pytest tests/unit/test_security.py -v` — full file, to confirm nothing
  else in the suite regresses.

## Risks and unknowns

- Passlib's exception hierarchy has other subclasses (e.g. for a hash that matches a
  known scheme's prefix but is internally malformed) that this fix does not touch,
  since neither the issue nor my reproduction produced one — I have only exercised the
  "unrecognized scheme" path. If the build turns up a second exception type from the
  same call site, I will treat that as a new finding to flag in the PR rather than
  silently widening the except clause beyond what this plan and its evidence cover.
- The passlib/bcrypt version-detection warning I noted as unrelated noise in my Unit 2
  repro comment is expected to still appear after the fix; it is not part of this
  issue and I am not suppressing it.

## Deviations

Nothing changed; the plan held. The build was exactly the two steps named in the
Approach section: a `try/except UnknownHashError: return False` around the existing
`pwd_context.verify(...)` call in `verify_password`, and removing the
`@pytest.mark.xfail` marker from `test_verify_with_wrong_hash_format`. No other file
was touched, no other passlib exception type showed up while building (so the
"if a second exception type turns up" risk noted above never materialized), and the
full `tests/unit/test_security.py` suite (25 tests) passes with no regressions. I also
ran `make lint` (passes) and `make typecheck`; typecheck fails, but on a pre-existing
`numpy` stub/mypy-version incompatibility inside `numpy/__init__.pyi` that has nothing
to do with `core/security.py` — running `mypy core/security.py` directly (isolating the
changed file) reports "Success: no issues found," confirming the failure is
environmental, not something this change introduced.
