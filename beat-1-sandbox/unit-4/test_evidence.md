# Test Evidence

## Unit 2 Reproduction Re-run

### Before (on main with --runxfail):

```bash
$ pytest tests/unit/test_security.py -k "test_verify_with_wrong_hash_format" -v --runxfail
=================================== test session starts ===================================
platform linux -- Python 3.11.15, pytest-9.1.1, pluggy-1.6.0
collected 25 items / 24 deselected / 1 selected

tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format FAILED [100%]

======================================== FAILURES =========================================
___________________ TestSecurity.test_verify_with_wrong_hash_format ___________________
core/security.py:37: in verify_password
    return bool(pwd_context.verify(plain_password, hashed_password))
/home/rociodv/anaconda3/envs/ai201/lib/python3.11/site-packages/passlib/context.py:1132: in identify_record
>   raise exc.UnknownHashError("hash could not be identified")
E   passlib.exc.UnknownHashError: hash could not be identified
============================= 1 failed, 24 deselected in 0.52s =============================
```

After (on branch fix/72-verify-password-hash):

```bash
$ pytest tests/unit/test_security.py -k "test_verify_with_wrong_hash_format" -v
=================================== test session starts ===================================
platform linux -- Python 3.11.15, pytest-9.1.1, pluggy-1.6.0
collected 25 items / 24 deselected / 1 selected

tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format PASSED [100%]
============================= 1 passed, 24 deselected in 0.58s =============================
```

## Repository Verification Checks

`make test-unit`

```bash
.venv/bin/pytest tests/unit -v -m unit
===================================== 376 passed, 52 xfailed, 3 warnings in 9.37s ======================================
```

`make test-integration`

```bash
.venv/bin/pytest tests/integration -v -m integration
================================================ no tests ran in 0.16s =================================================
make: *** [Makefile:43: test-integration] Error 5
(Note: Expected; Path Review currently has 0 integration tests defined)
```

`make lint`

```bash
.venv/bin/ruff check .
All checks passed!
```

`make typecheck`

```bash
.venv/bin/mypy api/ core/ ingestion/ rag/ agent/ safety/
Success: no issues found in 76 source files
```
