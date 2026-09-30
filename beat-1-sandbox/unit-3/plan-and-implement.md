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

rdv-chio

**Plan comment**

[Link to the comment where you posted your plan on the issue. Use the comment's own
permalink. **Then paste the text of that comment underneath the link** — the pasted text is
what this field is graded on, so copy across what you actually posted.]

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5899173263

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

---

## Your branch

**Branch**

fix/72-verify-password-hash

**Evidence**

Before (reproduction run exhibiting the failure under `--runxfail`):

```Plaintext
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

After (re-running against the built change on the branch):

```Plaintext
$ pytest tests/unit/test_security.py -k "test_verify_with_wrong_hash_format" -v
=================================== test session starts ===================================
platform linux -- Python 3.11.15, pytest-9.1.1, pluggy-1.6.0
collected 25 items / 24 deselected / 1 selected

tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format PASSED [100%]
============================= 1 passed, 24 deselected in 0.58s =============================

$ pytest tests/unit/test_security.py -v
=================================== test session starts ===================================
platform linux -- Python 3.11.15, pytest-9.1.1, pluggy-1.6.0
collected 25 items

tests/unit/test_security.py::TestSecurity::test_hash_password_returns_bcrypt_hash PASSED [  4%]
tests/unit/test_security.py::TestSecurity::test_verify_password_correct PASSED          [  8%]
tests/unit/test_security.py::TestSecurity::test_verify_password_incorrect PASSED        [ 12%]
tests/unit/test_security.py::TestSecurity::test_verify_password_case_sensitive PASSED    [ 16%]
tests/unit/test_security.py::TestSecurity::test_hash_same_password_different_hash PASSED [ 20%]
tests/unit/test_security.py::TestSecurity::test_create_access_token_returns_string PASSED [ 24%]
tests/unit/test_security.py::TestSecurity::test_create_access_token_is_jwt PASSED       [ 28%]
tests/unit/test_security.py::TestSecurity::test_decode_access_token_valid PASSED        [ 32%]
tests/unit/test_security.py::TestSecurity::test_decode_access_token_invalid_token PASSED [ 36%]
tests/unit/test_security.py::TestSecurity::test_decode_access_token_malformed PASSED     [ 40%]
tests/unit/test_security.py::TestSecurity::test_decode_access_token_empty_string PASSED  [ 44%]
tests/unit/test_security.py::TestSecurity::test_roundtrip_token_with_data PASSED        [ 48%]
tests/unit/test_security.py::TestSecurity::test_create_access_token_with_custom_expiry PASSED [ 52%]
tests/unit/test_security.py::TestSecurity::test_access_token_includes_expiration PASSED  [ 56%]
tests/unit/test_security.py::TestSecurity::test_password_hash_different_for_different_passwords PASSED [ 60%]
tests/unit/test_security.py::TestSecurity::test_verify_password_with_empty_strings PASSED [ 64%]
tests/unit/test_security.py::TestSecurity::test_verify_password_with_special_characters PASSED [ 68%]
tests/unit/test_security.py::TestSecurity::test_token_with_empty_data PASSED             [ 72%]
tests/unit/test_security.py::TestSecurity::test_token_with_special_characters_in_data PASSED [ 76%]
tests/unit/test_security.py::TestSecurity::test_token_with_unicode_data PASSED           [ 80%]
tests/unit/test_security.py::TestSecurity::test_hash_password_long_input PASSED          [ 84%]
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format PASSED     [ 88%]
tests/unit/test_security.py::TestSecurity::test_token_tampering_detection PASSED         [ 92%]
tests/unit/test_security.py::TestSecurity::test_create_token_consistency PASSED          [ 96%]
tests/unit/test_security.py::TestSecurity::test_password_with_whitespace PASSED         [100%]

=================================== 25 passed in 4.80s ====================================
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

20/20

**Package analysis**

pkg-20: The gold label was reject, and our rubric decided reject (failed: policy_compliance). In pkg-20, the candidate authored a high-quality, bounded plan that engaged with maintainer direction. However, ghostty's stated repository contribution policy explicitly requires disclosing all AI usage. Because the candidate draft comment omitted the required AI disclosure statement, our rubric evaluated policy_compliance as fail, successfully catching the critical thread-convention floor canary.

**Check rationale**

"| diagnosis_grounded | Plan's stated cause vs reproduction evidence & control runs | The stated root cause directly accounts for and is consistent with the reproduction evidence, control runs, and observed symptoms. The diagnosis must not contradict control data or blame a component already shown to work. | required |"

This check was designed after inspecting calib-03, where a polished plan adopted a maintainer's speculative key-binding diagnosis even though the reproduction evidence's timing matrix demonstrated that the delay persisted with no pager in the loop and that seeks were instantaneous with bindings unchanged. Demanding that the diagnosis directly account for control runs and observed symptoms prevents false accepts where speculative causes contradict empirical evidence.

**Trade-offs**

By making policy_compliance a required check that strictly enforces disclosure constraints from the repo's contribution policy, we guaranteed catching mandatory policy walls like pkg-20. The trade-off is that any candidate package in a repository with strict AI guidelines will be rejected immediately if disclosure is missing, even if the technical implementation plan itself is flawless. Because our procedure explicitly pairs this check with thread and repo fact inspection, it protects against maintainer friction upstream while maintaining a 20/20 agreement score across all categories.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
