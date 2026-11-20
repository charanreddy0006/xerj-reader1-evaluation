# Evaluation Methodology

## 1. Purpose

This evaluation documents observable XERJ Reader-1 behavior when answering questions about invoice documents. It is exploratory and intended to identify cases for follow-up, not estimate general system performance. This methodology is a retrospective account based on the surviving repository records; no preregistration or separately versioned protocol is present.

## 2. Test environment

The source methodology records:

- **Platform:** XERJ Reader-1 Playground
- **Document type:** Invoice
- **Document data:** Synthetic
- **Answer type:** Auto unless otherwise specified
- **Approach:** Manual, controlled question-based evaluation

The repository does not record a software version, evaluation date, or evaluator identity. Experiment 2's synthetic invoice is preserved. The original preloaded invoice and complete original test-case record for Experiment 1 were not preserved and cannot be reconstructed from the remaining records.

## 3. Test procedure

For each test, the evaluator established an expected answer from the source document, submitted a question, recorded the output, inspected any formula/program and evidence spans available, recorded confidence and latency where present, compared the response with the expected result, and assigned a classification.

The custom invoice's expected answers were independently calculated or read from its source text and are preserved with the test case. The preloaded invoice and complete Experiment 1 case record were not preserved. Some CSV question fields are shorthand labels; full prompts are preserved for the custom experiment, while the original Experiment 1 prompts are unavailable. Exact evidence-span checking is documented for selected cases, not for every test.

## 4. Evaluation categories

The CSV categories include direct extraction, date and identifier extraction, address extraction, arithmetic, quotation, cross-field reasoning, document reasoning, contextual interpretation, refusal, hallucination resistance, multi-field extraction, and robustness/direct extraction. Coverage is uneven and narrow; labels describe test intent, not breadth of validation.

## 5. Classification rules

- **PASS:** Expected answer correctly returned.
- **FAIL:** Incorrect answer or refusal when information is explicitly present.
- **INCONCLUSIVE:** Internal error, platform failure, unusable output, or a test whose premise does not support the intended evaluation.
- **OBSERVATION:** Correct value with an unusual answer type or other noteworthy behavior.

Final classifications in the CSV are preserved. T09 is recorded PASS although its correct currency value was labeled Text, which also fits the general OBSERVATION definition; the logged classification is retained and the mismatch disclosed. T22 is recorded as PASS under the documented conservative-abstention rationale because the document did not explicitly explain the purpose of the purchase-order number; the requested explanation was not returned. T23 and T24 are INCONCLUSIVE because the requested values were present despite the test premises claiming absence. See the [report](final-evaluation-report.md#classification-criteria).

Refusal of information documented as present is treated as FAIL; an expected refusal for information recorded as absent is treated as PASS. Internal errors are INCONCLUSIVE, separate from answer-quality outcomes. Tests with a false premise are also INCONCLUSIVE because they cannot evaluate the intended capability.

## 6. Limitations

- 25 planned slots are too few for statistically representative conclusions.
- The evaluation uses invoice documents and only two invoice contexts.
- The custom invoice is synthetic; the original preloaded source and complete original test-case record are not retained in this repository.
- Testing is manual, and results may depend on wording, document layout, and platform conditions.
- Confidence values are system-reported and not independently calibrated.
- Latency is missing for some rows; internal errors may be transient and their causes are unknown.
- T23 and T24 do not measure hallucination resistance because their premises were invalid.
- Results do not establish performance on other document types or general accuracy, reliability, or calibration.

## 7. Responsible testing

The source records state that synthetic data was used and no confidential financial, personal, or customer documents were uploaded.

