# Solution Approach: HHS/simpler-grants-gov#6192

## Analysis

The issue screenshot demonstrates that when a user inputs an out-of-range date like `01/01/275760` into a date field with MM/DD/YYYY helper text, the backend JSON schema validator rejects it as an invalid ISO date and produces the error string:
`'275760-01-01' is not a 'date'`

In the frontend pipeline (`frontend/src/utils/applyForm/applyFormUtils.ts`), error strings are processed by `validationWarningOverrides`:
```typescript
const validationWarningOverrides = (
  message: string,
  fieldName: string,
  title?: string | null,
) => {
  const formattedTitle = title ? title.replace("?", "") : "Field";
  return message
    .replace(fieldName, formattedTitle)
    .replace(/'/g, "")
    .replace("[] should be non-empty", `${formattedTitle} is required`)
    .replace("is a required property", "is required");
};
```

This behavior was reproduced locally and posted to issue #6192 (comment 6590808029). Running `repro.test.ts` on commit `e6bb081e7739f9da7a1e35b3b6e7d94d5a2fcade` yields this verbatim failure:

```Plaintext
● Issue 6192 Reproduction › formats date validation errors to specify MM-DD-YYYY format

    expect(received).toMatch(expected)

    Expected pattern: /MM-DD-YYYY/i
    Received string:  "275760-01-01 is not a date"
```

Currently, `validationWarningOverrides` strips single quotes, resulting in `"275760-01-01 is not a date"`. It fails the acceptance criteria because the message presents dates in `YYYY-MM-DD` format rather than `MM-DD-YYYY`.

## Proposed Solution

Target format: Standardize explicitly on `MM-DD-YYYY` across all date validation feedback to satisfy the acceptance criterion ("Validation error message on date fields reads back in MM-DD-YYYY format").

Update `validationWarningOverrides` in `frontend/src/utils/applyForm/applyFormUtils.ts` to transform date validation error strings matching the pattern `is not a 'date'` or `is not a date`:

1. Match the ISO date format pattern `/'?(\d{4})-(\d{2})-(\d{2})'? is not a '?date'?/i`.

2. Extract the year (`$1`), month (`$2`), and day (`$3`) components and reorder them into `MM-DD-YYYY` (`$2-$3-$1`).

3. Return the formatted message: `"${month}-${day}-${year} is not a valid date. Please use MM-DD-YYYY format."`

4. If the message matches a generic date format error (e.g., `does not match format "date"`), replace it with: `"${formattedTitle} must be a valid date in MM-DD-YYYY format."`

### Scope boundaries:

* Confined strictly to `validationWarningOverrides` in `frontend/src/utils/applyForm/applyFormUtils.ts`.

* No backend schemas, API models, or other form field overrides will be modified.

### Deviations Recorded During Build

* Expanded the year regex capture group from `\d{4}` to `\d{4,}` to handle 6-digit years (such as `275760` from the reproduction screenshot) without leaving unmatched leading digits.

* Standardized the generic date format check to match `must match format "date"`, prefixing the warning with `${formattedTitle}`.

## Implementation Plan

1. Unit Test Updates (`frontend/src/utils/applyForm/applyFormUtils.test.ts`):

    * Add test cases under `describe("formatValidationWarning")`:

        * Test input: `formatValidationWarning("project_end_date", "'275760-01-01' is not a 'date'", "Project End Date")`

        * Assertion: Expect the result to be `"01-01-275760 is not a valid date. Please use MM-DD-YYYY format."`

        * Test input for generic date error: `formatValidationWarning("project_end_date", 'must match format "date"', { title: "Project End Date" })`

        * Assertion: Expect result to be `"Project End Date must be a valid date in MM-DD-YYYY format."`

2. Implement Transformation in `applyFormUtils.ts`:

    * Add regex pattern matching in `validationWarningOverrides` to convert ISO date strings into `MM-DD-YYYY` and instruct the user on the required `MM-DD-YYYY` format.

3. Verification:

    * Run `npm --prefix frontend test -- applyFormUtils.test.ts` to confirm 100% test pass.

    * Run `npm --prefix frontend run lint` to verify clean linting.
