`fix(security): catch UnknownHashError in verify_password to fail closed (#72)`

# Summary

In `core/security.py`, `verify_password` directly delegates hash verification to `pwd_context.verify()`. When given a malformed hash or an unrecognized hash format prefix, `passlib` raises an unhandled `UnknownHashError`. This fix catches `UnknownHashError` within `verify_password` and returns `False`, failing closed securely rather than propagating an unhandled 500 error exception.

## Issue
Closes #72

## Changes

* In `core/security.py`: Import `UnknownHashError` from `passlib.exc` and wrap `pwd_context.verify()` inside a `try/except UnknownHashError:` block that returns `False`.

* In `tests/unit/test_security.py`: Remove `@pytest.mark.xfail` from `test_verify_with_wrong_hash_format` so it executes and passes as an active regression test.

## Testing

[x] Unit tests pass locally (`make test-unit`)

[ ] Integration tests pass locally (`make test-integration`)

[x] Linters pass locally (`make lint`)

[x] Type checks pass locally (`make typecheck`)

[ ] CI workflow passes

## Repro Steps Before & After

* Before: `pytest tests/unit/test_security.py -k "test_verify_with_wrong_hash_format" -v --runxfail` failed with `passlib.exc.UnknownHashError`.

* After: `pytest tests/unit/test_security.py -k "test_verify_with_wrong_hash_format" -v` passes (1 passed, 24 deselected).

* Full unit test suite passes: 376 passed, 52 xfailed, 0 unexpected failures.

## Screenshots / Demo

N/A - Internal security authentication utility fix; no visual UI changes.

## Notes for Reviewers

* AI Disclosure: I used Google Gemini and Claude Code to assist in analyzing the issue, authoring the plan, and drafting the pull request description.

* Scope matches the approved plan with no deviations: changes are strictly confined to `core/security.py` and the existing unit test.