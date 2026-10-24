# Reproduction Report: HHS/simpler-grants-gov#6192

## Environment Setup

- Operating System: Linux (WSL2 / Ubuntu 22.04 on Windows)
- Node.js runtime: v22.23.3 (executed on v22.23.3; project pins v24.19.0 in frontend/.nvmrc)
- Target repository: HHS/simpler-grants-gov
- Commit SHA: e6bb081e7739f9da7a1e35b3b6e7d94d5a2fcade
- Working directory: frontend/

## Reproduction Scope

This reproduction isolates the frontend error formatting pipeline (`src/utils/applyForm/applyFormUtils.ts`) via a focused unit test. It does not spin up the full backend or run an end-to-end browser integration flow; rather, it isolates the translation logic that receives backend validation error strings and formats them for display in the UI.

## Steps to Reproduce

1. Navigate to the frontend workspace:

`cd frontend`

2. Create `repro.test.ts` to exercise `formatValidationWarning` using the exact backend validation error string seen in the issue screenshot (`'275760-01-01' is not a 'date'`):

```typescript
   import { formatValidationWarning } from "src/utils/applyForm/applyFormUtils";

   describe("Issue 6192 Reproduction", () => {
     it("formats date validation errors to specify MM-DD-YYYY format", () => {
       // Simulating the exact backend error from the screenshot
       const rawBackendError = "'275760-01-01' is not a 'date'";
       const formatted = formatValidationWarning("project_end_date", rawBackendError);
       
       // Acceptance Criteria: Validation error message reads back in MM-DD-YYYY format
       expect(formatted).toMatch(/MM-DD-YYYY/i);
     });
   });
```

3. Run the reproduction test to verify that passing the backend error string to the formatter fails to produce the expected `MM-DD-YYYY` pattern:

```npm test -- repro.test.ts```

## Reproduction Evidence

- Verbatim Test Runner Output:

```Plaintext
> frontend@0.1.0 test
> jest --ci --coverage repro.test.ts

  Using tsconfig file: tsconfig.build.json
 FAIL  ./repro.test.ts (9.759 s)
  Issue 6192 Reproduction
    ✕ formats date validation errors to specify MM-DD-YYYY format (12 ms)

  ● Issue 6192 Reproduction › formats date validation errors to specify MM-DD-YYYY format

    expect(received).toMatch(expected)

    Expected pattern: /MM-DD-YYYY/i
    Received string:  "275760-01-01 is not a date"

       9 |
      10 |      // Acceptance Criteria: Validation error message reads back in MM-DD-YYYY format
    > 11 |      expect(formatted).toMatch(/MM-DD-YYYY/i);
         |                        ^
      12 |    });
      13 | });

      at Object.toMatch (repro.test.ts:11:23)

Test Suites: 1 failed, 1 total
Tests:       1 failed, 1 total
Snapshots:   0 total
Time:        52.931 s
Ran all test suites matching repro.test.ts.
```

- Current Observed Behavior:

As evidenced in the Issue #6192 description screenshot (where a user entered `01/01/275760` into a field with helper text `"Enter the date in the format MM/DD/YYYY."`), the backend validator evaluates against ISO standard and returns `"'275760-01-01' is not a 'date'"`.
In `frontend/src/utils/applyForm/applyFormUtils.ts`, `validationWarningOverrides` strips single quotes, resulting in `"275760-01-01 is not a date"`. It does not convert or format the message to specify the expected `MM-DD-YYYY` pattern.

- Expected Behavior:
Per acceptance criteria of issue #6192:

  - "Validation error message on date fields reads back in MM-DD-YYYY format".
  - The validation feedback should explicitly instruct the user using the `MM-DD-YYYY` format to align with field input guidance.
