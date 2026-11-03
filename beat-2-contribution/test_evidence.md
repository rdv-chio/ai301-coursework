# Test Evidence: HHS/simpler-grants-gov#6192

## 1. Reproduction Output (Before Fix)

Running `repro.test.ts` on commit `e6bb081e7739f9da7a1e35b3b6e7d94d5a2fcade` demonstrates that passing raw backend error `'275760-01-01' is not a 'date'` strips quotes but fails to convert the ISO format to `MM-DD-YYYY`:

```text
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

## 2. Test Suite Output (After Fix)

Running `applyFormUtils.test.ts` verifies both ISO date error transformation and generic date format warnings:

```Plaintext
> frontend@0.1.0 test
> jest --ci --coverage applyFormUtils.test.ts

  Using tsconfig file: tsconfig.build.json
 PASS  src/utils/applyForm/applyFormUtils.test.ts (8.918 s)
  ✓ pushes direct warnings for uiSchema fields
  ✓ handles nested uiSchema sections (1 ms)
  shapeFormData
    ✓ should shape form data to the form schema (197 ms)
    ✓ should shape nested form data (57 ms)
  getFieldSchema
    ✓ should return the schema for a single field with only a definition (1 ms)
    ✓ should merge the schema from the uiFieldObject with the form schema (1 ms)
  pruneEmptyNestedFields
    ✓ returns flat object with undefined fields removed (1 ms)
    ✓ returns empty object if passed an empty object or object with only undefined fields
    ✓ prunes all empty objects and objects containing only undefined properties (1 ms)
    ✓ removes nested objects containing only undefined properties or objects with only undefined properties
    ✓ removes empty nested objects from within arrays
    ✓ removes empty array values from arrays (1 ms)
    ✓ removes empty array values from nested arrays
  getFieldNameForHtml
    ✓ returns correct field name based on definition, removing properties and adding delimiter
    ✓ returns correct field name based on schema (1 ms)
  processFormSchema
    ✓ calls extricateConditionalValidationRules with formSchema.properties (1 ms)
    ✓ defaults to empty properties when formSchema.properties is undefined (1 ms)
    ✓ calls mergeAllOf with the properties returned from extricateConditionalValidationRules (1 ms)
    ✓ returns the expected combination of original schema and merged properties (1 ms)
    ✓ rethrows if a processor throws (62 ms)
  condenseFormSchemaProperties
    ✓ leaves an object with no 'properties' unchanged (1 ms)
    ✓ brings contents of 'properties' attributes up one level (1 ms)
  getRequiredProperties
    ✓ returns an empty array for a schema with no required fields
    ✓ returns a list of all unconditionally required property paths within a schema
  jsonPointerToPath
    ✓ converts pointer to JSON path (1 ms)
  getFieldsForNav
    ✓ returns nav items for schema
    ✓ returns empty array for non-array input
  getKeyParentPath
    ✓ returns combined and cleaned path
    ✓ returns key if no parent
  isFieldRequired
    ✓ returns true if definition is in requiredFields
    ✓ returns false if not required (1 ms)
    ✓ handles array of definitions
  buildWarningTree
    ✓ returns formatted warnings for matching fields (2 ms)
    ✓ returns empty array if no matching warnings
    ✓ handles nested fieldList nodes (1 ms)
    ✓ handles fieldList nested under section (3 ms)
    ✓ preserves multiple row-aware warnings for the same FieldList child field (1 ms)
    ✓ builds a row-aware htmlField for FieldList child warnings
  pointerToFieldName
    ✓ changes a pointer to a field name
  addPrintWidgetToFields
    ✓ converts regular fields to Print widget
    ✓ converts AttachmentArray widget to PrintAttachment widget
    ✓ handles sections with nested fields
    ✓ handles nested sections (1 ms)
    ✓ handles mixed field and section types
    ✓ handles empty ui schema
    ✓ preserves other field properties (1 ms)
    ✓ preserves empty object array entries so FieldList validation can return child-level errors
    ✓ handles fieldList with nested fields (1 ms)
    formatValidationWarning
      ✓ formats required property messages using the schema title
      ✓ formats non-empty array messages using the schema title
      ✓ falls back to a generic field label when no title exists (1 ms)
      ✓ formats date validation errors to MM-DD-YYYY format when given an ISO date string
      ✓ formats generic date validation errors to specify MM-DD-YYYY format
    getHtmlFieldForWarning
      ✓ builds a row-aware html field for indexed FieldList warnings
      ✓ falls back to entry 0 when the FieldList warning is not indexed
      ✓ falls back to standard html field generation for non-FieldList definitions (1 ms)
    getFieldListLabelFromDefinition
      ✓ finds a top-level FieldList label
      ✓ finds a nested FieldList label inside a section
      ✓ returns undefined when no matching FieldList exists (1 ms)
    findValidationErrors
      ✓ returns multiple row-aware warnings for the same FieldList child definition
      required field inside a missing nested object
        ✓ shows only the field title when the parent object has no title
        ✓ keeps the parent title as a prefix when the parent sets one (1 ms)

Test Suites: 1 passed, 1 total
Tests:       62 passed, 62 total
Snapshots:   0 total
Time:        50.324 s
Ran all test suites matching applyFormUtils.test.ts.
```

## 3. Ship Manifest Checks

### Check 1: Frontend Tests

Command: `npm --prefix frontend test -- applyFormUtils.test.ts`
Result: 62 passed, 1 total suite passed.

### Check 2: Frontend Lint

Command: `npm --prefix frontend run lint`
Result: Clean pass (0 errors).

### Excluded Check: Full Frontend Test Suite

Command: `npm --prefix frontend test`
Reason: Fails on main with 2 pre-existing failures (`withFeatureFlag.test.tsx` and `generalUtils.test.ts`), excluded per ship-gate rules.
