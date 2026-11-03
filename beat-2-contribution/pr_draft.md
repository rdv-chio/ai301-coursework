## Summary

Fixes #6192

Resolves date validation error messages in application forms displaying raw backend ISO `YYYY-MM-DD` strings rather than matching the user-facing `MM-DD-YYYY` format.

## Changes proposed

- In `frontend/src/utils/applyForm/applyFormUtils.ts`, update `validationWarningOverrides` with regex pattern matching (`/'?(\d{4,})-(\d{2})-(\d{2})'? is not a '?date'?/i`) to reformat ISO date strings into `MM-DD-YYYY` with explicit user guidance (`01-01-275760 is not a valid date. Please use MM-DD-YYYY format.`).
- Handle generic schema date format errors (`must match format "date"`) to output `${formattedTitle} must be a valid date in MM-DD-YYYY format.`.
- In `frontend/src/utils/applyForm/applyFormUtils.test.ts`, add unit tests verifying that both specific ISO dates and generic date errors format to `MM-DD-YYYY`.

## Context for reviewers

When backend JSON schema validation rejects invalid dates (such as year `275760` from issue screenshot), it emits `'YYYY-MM-DD' is not a 'date'`. Previously, `validationWarningOverrides` stripped quotes but left the ISO string unchanged (`275760-01-01 is not a date`), causing confusion against form input guidance. The regex uses `\d{4,}` to properly support extended year digits.

## Validation steps

1. Run frontend unit tests:

```bash
npm --prefix frontend test -- applyFormUtils.test.ts
```

Verified 62 passed tests, including new date formatting tests.

2. Run frontend linter:

```bash
npm --prefix frontend run lint
```

Verified 0 errors.

## AI Assistance Disclosure

AI assistance (Claude Code and Gemini) was used during this workflow to assist with local reproduction setup, capture group regex boundary verification, and linter check validation.


🤖 Generated with [Claude Code](https://claude.com/claude-code)
