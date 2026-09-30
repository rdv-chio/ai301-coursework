### Plan: Handle malformed password hashes by failing closed (#72)

Following up on the reproduction report above:

**Diagnosis:**
`verify_password` in `core/security.py` delegates directly to `pwd_context.verify()`, which raises `passlib.exc.UnknownHashError` when the stored hash string is malformed or uses an unrecognized algorithm prefix.

**Proposed Changes:**
1. Catch `UnknownHashError` from `passlib.exc` in `verify_password` in `core/security.py` and return `False` to fail closed securely.
2. Remove the `@pytest.mark.xfail` decorator from `test_verify_with_wrong_hash_format` in `tests/unit/test_security.py` so the test runs and passes as an active regression test.
3. Keep changes strictly bounded to `core/security.py` and the unit test; no hashing schemes or token functions will be altered.

**Verification:**
Re-run `pytest tests/unit/test_security.py -v` to confirm that `test_verify_with_wrong_hash_format` passes alongside all existing security unit tests.

*Note: I used Google Gemini and Claude Code to assist in analyzing the issue and drafting this plan.*