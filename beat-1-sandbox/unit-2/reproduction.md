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

rdv-chio

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5863245560

Hi maintainers!

I would like to investigate and reproduce this issue regarding `verify_password` raising `UnknownHashError` on malformed stored hashes. I will set up the local environment and test suite, and follow up with a reproduction report shortly.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5863392140

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

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

19/20

**Package analysis**

pkg-03: The gold label was accept, but our rubric produced reject (failed: policy_compliance). In pkg-03, the repository facts stated a standard contribution policy with no explicit AI restriction, and the candidate did not mention AI assistance. However, our evaluator applied strict scrutiny to policy_compliance, triggering a false-negative rejection on an otherwise clean reproduction package.

**Check rationale**

"| outcome_documented | Repro report observed vs expected behavior | Displays the verbatim output, stack trace, error log, or symptom observed, and explicitly notes what should have happened instead (or confirms failure under test). | required |"

This check was designed after evaluating calib-03 during calibration, where the candidate typed a syntax typo (":" instead of "=") in an HCL key-value pair and encountered a parser error, yet claimed the runtime panic was successfully reproduced. Requiring the verbatim output, stack trace, or symptom to explicitly document the observed behavior against the expected behavior prevents approving false positives where user error masks the reported defect.

**Trade-offs**

By making policy_compliance a required check with strict adherence checks, we ensured our rubric caught the single mandatory disclosure-wall package in the eval set (achieving 1/1 on the disclosure floor). The trade-off was Sonnet becoming overly cautious on pkg-03, rejecting it even though the gold label was accept. Because agreement was 19/20 (comfortably clearing the 18/20 bar) and all categories passed, we kept the check strict rather than loosening it and risking the disclosure canary.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
