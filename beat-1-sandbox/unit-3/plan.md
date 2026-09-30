# Plan: Catch `UnknownHashError` and Fail Closed in `verify_password` (#72)

## Diagnosis
In `core/security.py`, `verify_password(plain_password, hashed_password)` directly delegates verification to `pwd_context.verify(plain_password, hashed_password)`. When `hashed_password` is malformed or uses an unrecognized algorithm prefix, `passlib` raises `passlib.exc.UnknownHashError` rather than returning `False`. Because this exception is unhandled, any authentication flow encountering a malformed stored hash crashes with an unhandled exception rather than failing closed.

## Scope
- **In-scope**:
  - Catch `passlib.exc.UnknownHashError` (and any related hash format exceptions from passlib) within `verify_password` in `core/security.py` and return `False`.
  - Update `tests/unit/test_security.py` to remove `@pytest.mark.xfail` from `test_verify_with_wrong_hash_format` so it executes and passes as an active regression test.
- **Not-in-scope**:
  - Modifying token creation or decoding logic in `core/security.py`.
  - Changing password hashing logic or switching hashing algorithms away from bcrypt.
  - Adding new dependencies or altering authentication endpoints in the API layer.

## Approach
1. In `core/security.py`:
   - Import `UnknownHashError` from `passlib.exc`.
   - Wrap the `pwd_context.verify(...)` call in `verify_password` with a `try/except UnknownHashError:` block.
   - Return `False` when `UnknownHashError` is encountered.
2. In `tests/unit/test_security.py`:
   - Remove the `@pytest.mark.xfail(...)` decorator from `test_verify_with_wrong_hash_format`.
   - Ensure the test asserts that `verify_password("password", "not_a_valid_bcrypt_hash")` evaluates to `False`.

## Test Plan
- Re-run the reproduction test:
```bash
  pytest tests/unit/test_security.py -k "test_verify_with_wrong_hash_format" -v
```

Expected outcome: The test transitions from `FAILED` (under `--runxfail`) to `PASSED`.

Run all security unit tests to ensure no regressions:

```bash
pytest tests/unit/test_security.py -v
```

Expected outcome: All 25 tests pass with 0 failures and 0 xfails.

## Risks and Unknowns

Low risk. The change strictly adheres to standard security fail-closed principles. No API contract or database schemas are altered.

## Deviations

Nothing changed; the plan held as designed.