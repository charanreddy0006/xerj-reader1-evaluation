# Future Evaluation Plan

This document is a proposed roadmap for future work. It describes tests that have not been conducted and is separate from the completed 25-slot evaluation documented in the [final report](final-evaluation-report.md).

## Phase A - Service reliability

Resume testing only when the Playground is behaving normally.

- Retry a previously failed request once.
- Record platform errors and request IDs.
- Avoid repeated retries.
- Separate infrastructure failures from model errors.

## Phase B - Extraction

Use controlled synthetic documents to test:

- Names and addresses
- Dates
- Invoice and purchase-order identifiers
- Currency values and percentages
- Multi-field questions

## Phase C - Financial reasoning

Test calculations in isolation:

- Subtotal
- Discount
- VAT
- Shipping
- Final total and order of operations

## Phase D - Evidence grounding

Evaluate exact quotations, evidence-span accuracy, missing evidence, and conflicting source values.

## Phase E - Refusal behavior

Test both explicitly present and absent information. Distinguish false refusals from appropriate refusals, validate that each absence premise is true in the complete document, and check refusal consistency across wording variants.

## Phase F - Document complexity

Use synthetic documents containing tables, repeated fields, different date roles, multiple pages, irrelevant text, and ambiguous field labels.

## Phase G - Structured evaluation

As the test set grows, consider exact-match results, extraction by field, false and correct refusal rates, arithmetic performance, evidence-span accuracy, answer-type accuracy, confidence calibration, and platform errors. Define each denominator and gather a sufficiently larger, varied sample before calculating or presenting such measures.
