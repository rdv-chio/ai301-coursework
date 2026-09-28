### Reproduction Report: `verify_password` raises `UnknownHashError` on malformed hash (#72)

**Environment**
- OS: Linux (Ubuntu via WSL2, kernel 5.15)
- Python: 3.11.15
- Dependencies: pytest 9.1.1, passlib 1.7.4

**Steps to Reproduce**
1. Run pytest targeting the security unit test with `--runxfail` enabled:

   ```bash
   pytest tests/unit/test_security.py -k "test_verify_with_wrong_hash_format" -v --runxfail
    ```

# Observed Behavior
When `pwd_context.verify(plain_password, hashed_password)` executes against an unrecognized or invalid hash format, `passlib` raises an unhandled `passlib.exc.UnknownHashError` instead of returning `False`:

```Plaintext
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format - passlib.exc.UnknownHashError: hash could not be identified

core/security.py:37: in verify_password
    return bool(pwd_context.verify(plain_password, hashed_password))
/home/rociodv/anaconda3/envs/ai201/lib/python3.11/site-packages/passlib/context.py:1132: in identify_record
>   raise exc.UnknownHashError("hash could not be identified")
E   passlib.exc.UnknownHashError: hash could not be identified
```

# Expected Behavior
`verify_password` should handle or catch `UnknownHashError` and return `False`, failing closed securely rather than propagating an unhandled 500 error exception.