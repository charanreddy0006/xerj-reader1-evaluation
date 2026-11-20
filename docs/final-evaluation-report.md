# XERJ Reader-1 Evaluation

## Exploratory Evaluation Report

**Scope:** 25 planned test slots across two invoice experiments  
**Report date:** 20 November 2026  
**Evaluation date:** Not recorded in the source files  
**Evaluator:** Not identified in the source files

## Contents

1. [Executive summary](#1-executive-summary)
2. [System and evaluation overview](#2-system-and-evaluation-overview)
3. [Methodology](#3-methodology)
4. [Evaluation categories](#4-evaluation-categories)
5. [Overall results](#5-overall-results)
6. [Detailed test results](#6-detailed-test-results)
7. [Key findings](#7-key-findings)
8. [Failure analysis](#8-failure-analysis)
9. [Reliability and latency](#9-reliability-and-latency)
10. [Recommendations](#10-recommendations)
11. [Limitations](#11-limitations)
12. [Conclusion](#12-conclusion)
13. [Appendix](#13-appendix-classifications-and-records)

## 1. Executive summary

This report documents an exploratory evaluation of XERJ Reader-1 in the XERJ Reader-1 Playground. The 25 planned test slots cover direct invoice extraction, evidence grounding, arithmetic, cross-field reasoning, refusal behavior, answer typing, and request reliability. The records describe a preloaded invoice and a custom synthetic invoice; the preloaded source document is not included in the repository.

The evaluation log records 14 PASS, 7 FAIL, 4 INCONCLUSIVE, and 0 OBSERVATION outcomes. These are counts of recorded classifications, not an accuracy rate. All 25 slots have a row in the log, but four are inconclusive and two of those (T23-T24) had invalid test premises. Completion of a slot does not mean the intended behavior was successfully evaluated.

Successful cases include several direct extractions, evidence-backed identifier and value extraction, one correct refusal for information reported absent, and a due-date calculation from invoice date and payment terms. Observed issues include false refusals for fields reported as explicitly present, an incorrect total-payable calculation, a mistaken interpretation of dates with different roles as conflicting, and two internal-error test cases. T09 returned the right shipping amount but labeled it Text.

T22 is retained as PASS per the documented rationale: abstention was considered conservative because the invoice did not explicitly explain the purchase-order reference's purpose. This was not a successful answer to the contextual question. T23 and T24 remain INCONCLUSIVE because their "information absent" premise was contradicted by the source; their extractions do not establish performance on genuinely absent information.

Overall, the records show useful extraction and evidence behavior in some cases alongside material failure modes. This small, manually conducted evaluation cannot establish general accuracy, calibration, reliability, or comparative performance.

## 2. System and evaluation overview

The source records identify XERJ Reader-1 as an early-access document extraction and reasoning system accessed through its Playground. The evaluation observes responses to invoice questions about values, identifiers, dates, and contextual or calculated answers. No further implementation or deployment details are documented.

The objectives were to retrieve explicitly stated facts, observe source evidence, check financial and date reasoning, observe answering and abstention, and record answer-type labels and request errors. The methodology records manual, controlled question-based testing, synthetic invoice data, and Auto answer type unless otherwise specified.

Experiment 2's custom invoice and independent expected total are preserved in [the experiment document](../test-cases/experiment-02-custom-invoice.md). Experiment 1 used a preloaded invoice, but the original invoice and complete original test-case record were not preserved. The evaluation date, software version, and evaluator identity are not recorded.

## 3. Methodology

For each test, the evaluator established an expected result from the source, submitted a question, recorded the response, inspected formula/program output and evidence spans where available, and recorded confidence and latency where present. The response was compared with the expected result and classified. The raw row-level record is [results/evaluation-log.csv](../results/evaluation-log.csv).

The evaluation is exploratory and manual. Test slots share two invoice contexts and are not a randomized or statistically representative benchmark.

### Classification criteria

- **PASS:** Expected answer correctly returned.
- **FAIL:** Incorrect answer or refusal when information is explicitly present.
- **INCONCLUSIVE:** Internal error, platform failure, unusable output, or a test whose premise did not support the intended evaluation.
- **OBSERVATION:** Correct value with an unusual answer type or other noteworthy behavior.

Logged classifications are retained. T09 is recorded PASS although its correct currency value was labeled Text, a behavior that also fits the general OBSERVATION definition; the recorded status is preserved and the type mismatch is called out separately. T22 is a documented exception in interpretation: its PASS reflects conservative abstention where the document did not explicitly state the purchase-order number's purpose; the requested explanation was not answered. T23 and T24 remain INCONCLUSIVE because their targets were present, invalidating the test premise. See the [classification note](#13-appendix-classifications-and-records).

## 4. Evaluation categories

The CSV categories are grouped below for readability. A category label describes test intent, not broad coverage.

| Group | Logged categories and coverage |
|---|---|
| Direct extraction | Extraction, date and identifier extraction, address extraction, robustness/direct extraction; values, dates, IDs, contact details, address, and currency |
| Evidence grounding | Evidence spans are noted for selected successes; the log does not contain a complete evidence audit for every test |
| Calculation and reasoning | Arithmetic, cross-field reasoning, document reasoning, contextual interpretation; one total calculation, one due-date calculation, date roles, one PO context question |
| Refusal and answerability | Refusal, hallucination resistance, multi-field extraction; expected and false refusals, plus two invalid absence premises |
| Output and service behavior | Quotation, answer-type observation, and internal errors recorded under extraction/date extraction |

## 5. Overall results

Counts were recalculated from the 25 data rows in `results/evaluation-log.csv`.

| Classification | Count | Interpretation |
|---|---:|---|
| PASS | 14 | Recorded classifications; T09 and T22 have scoring caveats |
| FAIL | 7 | Incorrect result or false refusal under the logged expected outcome |
| INCONCLUSIVE | 4 | Two internal-error cases and two invalid-premise cases |
| OBSERVATION | 0 | No row has this classification |
| **Total slots** | **25** | Every planned slot has a CSV row, but not all yielded interpretable evidence for the intended question |

| Experiment | Slots | PASS | FAIL | INCONCLUSIVE | OBSERVATION |
|---|---:|---:|---:|---:|---:|
| Experiment 1 (preloaded invoice) | 17 | 12 | 3 | 2 | 0 |
| Experiment 2 (custom synthetic invoice) | 8 | 2 | 4 | 2 | 0 |
| **Total** | **25** | **14** | **7** | **4** | **0** |

T12 had two request attempts, both returning internal errors; its slot is counted once. T13 had an internal error. T23 and T24 are inconclusive because their target values were present. These counts are not an accuracy score: test design, scoring rationale, and invalid premises limit that interpretation.

## 6. Detailed test results

Confidence and latency reproduce logged values; "-" means none was recorded. Expected and observed values are abbreviated for readability; the CSV retains exact raw strings.

### Experiment 1 - preloaded invoice

| ID | Objective | Expected | Observed | Status | Conf. | ms | Note |
|---|---|---|---|---|---:|---:|---|
| T01 | Total due | EUR 12,480.00 | EUR 12,480.00 | PASS | - | - | Evidence grounded |
| T02 | Due date | 2026-11-30 | 2026-11-30 | PASS | - | - | Date normalized |
| T03 | Invoice number | INV-2026-00417 | INV-2026-00417 | PASS | - | - | Direct extraction |
| T04 | Purchase order | PO-7781 | PO-7781 | PASS | - | - | Direct extraction |
| T05 | Refuse absent CEO query | Decline | DECLINE ABSENT | PASS | - | - | Expected refusal |
| T14 | Invoice number | INV-2026-00417 | INV-2026-00417 | PASS | 1.00 | 214 | Exact evidence span |
| T15 | BIC | COBADEFFXXX | DECLINE ABSENT | FAIL | 0.98 | 254 | BIC reported present; false refusal |
| T16 | IBAN | DE89 3704 0044 0532 0130 00 | DE89 3704 0044 0532 0130 00 | PASS | 0.98 | 205 | Exact evidence span |
| T17 | Email | accounts@northwind-components.example | accounts@northwind-components.example | PASS | 1.00 | 232 | Exact evidence span |
| T18 | Seller address | Hafenstrasse 18, Hamburg, Germany | Hafenstrasse 18, 20457 Hamburg, Germany | PASS | 1.00 | 231 | Includes postal code; exact span |
| T19 | Seller, buyer, invoice number | All three fields | DECLINE ABSENT | FAIL | 0.98 | 229 | False refusal on multi-field request |
| T20 | Due date from date + terms | 2026-11-30 | 2026-11-30 | PASS | 0.99 | 284 | Correct DATE + DUR interpretation; evidence for both inputs |
| T21 | Conflicting dates | No conflict; distinct roles | YES | FAIL | 0.97 | 286 | Did not identify date roles or explain them |
| T22 | Purpose of PO number | PO identifies buyer's PO | DECLINE ABSENT | PASS* | 0.99 | 214 | Conservative-abstention rationale; question unanswered |
| T23 | Supposedly absent bank account | Information absent | DE89 3704 0044 0532 0130 00 | INCONCLUSIVE | 0.89 | 189 | Invalid premise: account present and extracted with evidence |
| T24 | Supposedly absent VAT ID | Information absent | DE 812 455 921 | INCONCLUSIVE | 0.99 | 186 | Invalid premise: VAT ID present and extracted with evidence |
| T25 | Invoice currency | EUR | EUR | PASS | 0.99 | 189 | Exact evidence span |

### Experiment 2 - custom synthetic invoice

| ID | Objective | Expected | Observed | Status | Conf. | ms | Note |
|---|---|---|---|---|---:|---:|---|
| T06 | Total payable | EUR 18,504.00 | EUR 18,530.00 | FAIL | 0.662 | - | Valid arithmetic, incorrect interpretation of invoice instructions |
| T07 | Discount amount | EUR 1,700.00 | DECLINE ABSENT | FAIL | 0.984 | 211 | False refusal |
| T08 | VAT amount | EUR 2,754.00 | DECLINE ABSENT | FAIL | 0.956 | 204 | False refusal |
| T09 | Shipping charge | EUR 450.00 | EUR 450.00 | PASS | 0.858 | 194 | Correct value labeled Text |
| T10 | Quote discount line | "Discount Amount: EUR 1,700.00" | DECLINE ABSENT | FAIL | 0.995 | 208 | False refusal despite explicit line |
| T11 | Invoice number | INV-TEST-002 | INV-TEST-002 | PASS | 0.99 | 198 | Exact source span |
| T12 | Seller | Apex Industrial Supplies | INTERNAL ERROR | INCONCLUSIVE | - | - | Two attempts; both errors |
| T13 | Due date | 19 December 2026 | INTERNAL ERROR | INCONCLUSIVE | - | - | No usable answer |

\* T22 retains the CSV's PASS and the rationale in `docs/findings.md`. Under a strict "expected answer returned" reading, the actual response did not provide the expected explanation. The classification reflects the evaluator's conservative-abstention judgment, not successful contextual interpretation.

## 7. Key findings

### Direct extraction and evidence

The log records successful extraction of values, dates, identifiers, an IBAN, email, seller address, and currency. Exact evidence spans are noted for selected cases. These examples show evidence-backed extraction occurred in this sample; they do not establish consistency across documents or fields. T18 included a postal code beyond the originally specified address components.

### False refusals on information reported present

Reader-1 declined requests for values the records report as present. Full custom-invoice prompts are preserved; for T15 and T19, the table shows the CSV's shorthand question field because the original prompt wording is unavailable.

| ID | Question recorded | Expected | Actual | Confidence | Latency |
|---|---|---|---|---:|---:|
| T07 | What is the discount amount applied to this invoice? | EUR 1,700.00 | DECLINE ABSENT | 0.984 | 211 ms |
| T08 | What is the VAT amount stated on this invoice? | EUR 2,754.00 | DECLINE ABSENT | 0.956 | 204 ms |
| T10 | Quote the exact line in the invoice that states the discount amount. | Discount Amount: EUR 1,700.00 | DECLINE ABSENT | 0.995 | 208 ms |
| T15 | BIC (CSV question field; full prompt unavailable) | COBADEFFXXX | DECLINE ABSENT | 0.98 | 254 ms |
| T19 | Seller name buyer name and invoice number (CSV question field) | All three fields extracted | DECLINE ABSENT | 0.98 | 229 ms |

These are false refusals in the tested document and scenarios, not a general failure rate. Confidence values are system-reported and are not independently calibrated probabilities.

### Calculation and contextual reasoning

T06 returned EUR 18,530 instead of EUR 18,504. The invoice specifies EUR 17,000 subtotal, subtracts EUR 1,700 discount, applies EUR 2,754 VAT to EUR 15,300 taxable amount, then adds EUR 450 shipping. The expected calculation is `17,000 - 1,700 + 2,754 + 450 = 18,504`. The finding describes the generated formula as mathematically valid but semantically inconsistent with the instructions.

T20 correctly derived the due date using invoice date and 30-day terms. T21 treated invoice and due dates as conflicting without establishing their different roles or providing the requested explanation. Each is one case; neither establishes a broad reasoning pattern.

### Answer type, abstention, and invalid premises

T09 returned the correct shipping amount but labeled it Text. T05 is a logged expected refusal for a CEO reported absent. T22 is recorded PASS for conservative abstention where the PO's purpose was not explicit; the question remained unanswered. T23 and T24 cannot evaluate hallucination resistance because the requested values were present.

### Request errors

T12 returned internal errors on two attempts, and T13 returned one internal error. The log marks both INCONCLUSIVE rather than answer-quality failures. The records do not establish the cause.

## 8. Failure analysis

| Issue | Expected | Observed | Relevance | Evidence boundary |
|---|---|---|---|---|
| Total-payable interpretation (T06) | Apply discount, VAT, shipping order; EUR 18,504 | EUR 18,530 | Financial totals can be wrong despite plausible component arithmetic | Finding reports a semantic formula error; system cause unknown |
| False refusals (T07, T08, T10, T15) | Extract or quote present information | DECLINE ABSENT | Refusal can block routine use when a value is present | Presence documented; no causal mechanism established |
| Multi-field refusal (T19) | Return seller, buyer, invoice number | DECLINE ABSENT | Combining fields may affect answerability in this instance | One observation only |
| Date roles (T21) | Recognize different roles and explain | YES, dates conflict | Value comparison without role interpretation can mislead | Program comparison described in findings; implementation cause unknown |
| Answer type (T09) | Currency value with suitable type | Correct amount labeled Text | Typed downstream use may be affected | One case; downstream effects not tested |
| Internal errors (T12, T13) | Usable answer or diagnosed service outcome | Internal errors | Cases could not be assessed for answer quality | Cause, duration, broader incidence undocumented |

These rows distinguish observed output from practical interpretation. Underlying model or platform mechanisms are not established by the available evidence.

## 9. Reliability and latency

Latency is recorded for 17 CSV rows; 8 rows have no value. Recorded numeric values range from 186 to 286 ms. This is a description of this sample, not a system-wide performance claim. T12 and T13 are the two internal-error slots; T12 had two failed attempts. T23 and T24 are inconclusive for invalid premises, not reliability failures.

The CSV does not contain request IDs. Earlier feedback notes preserve `req_fecaw4tt68vps3gg31gq4xht` for seller extraction and `req_7e56dmqy33vx62q78xxvfxrf` for due-date extraction. No error rate or latency percentile is calculated: the sample is small, some values are missing, and attempts are not independent.

## 10. Recommendations

### High priority

1. **Review refusals on present fields.** Reproduce discount, VAT, BIC, quotation, and multi-field cases with source text and returned evidence visible. Test controlled wording and placement variants.
2. **Review financial formula interpretation.** Test components independently, then the total. Verify discount base, tax base, and shipping order, and preserve formula and evidence for each run.
3. **Investigate internal errors.** Correlate T12/T13 request IDs with service logs if available. Use a documented retry policy and keep service errors separate from answer-quality outcomes.

### Improvements to investigate

4. **Check date-role reasoning.** Use explicit invoice, delivery, and due-date labels; score extraction separately from conflict interpretation.
5. **Check answer typing.** Include currency, percentage, date, and text outputs; score value normalization separately from type.
6. **Clarify classification policy.** Decide how conservative abstention on implicit contextual questions should be scored, then apply the policy consistently. Preserve T22's historical rationale.

### Additional testing

7. **Repair absence-test premises.** Confirm each target is absent from the complete uploaded document, including tables, footers, and account sections.
8. **Add document variety and controlled repetitions.** Use multiple synthetic layouts, field locations, and wording variants. Define denominators before reporting rates.
9. **Complete reproducibility records.** Preserve prompts, answer-type settings, run dates, version, evidence, formulas, latency, and request IDs, without changing existing raw logs.

## 11. Limitations

- This is an exploratory evaluation of 25 planned slots, not a statistically representative benchmark.
- It uses invoice documents; the custom invoice is synthetic, and the preloaded source is not preserved here.
- Tests are manual, share document contexts, and may depend on wording or layout.
- The original preloaded invoice and complete Experiment 1 test-case record were not preserved, so that experiment cannot be fully reproduced from this repository alone.
- T23 and T24 did not test absence because the requested values existed in the source.
- T22's PASS reflects a case-specific conservative-abstention rationale that differs from a strict reading of the general PASS definition.
- Confidence is system-reported and not independently calibrated. Latency is missing for some rows; error causes are unknown.
- The evaluation does not establish general accuracy, reliability, safety, calibration, or performance on other document types.

## 12. Conclusion

In this sample, XERJ Reader-1 returned several correct, evidence-supported extractions and correctly derived one due date from an invoice date and payment terms. The records also show false refusals for information reported present, one incorrect financial total, one mistaken date-conflict judgment, an answer-type mismatch, and two service-error slots. Two intended hallucination-resistance tests were invalid because the supposedly absent information was present.

These cases provide concrete follow-up topics for the XERJ team, particularly refusal behavior, financial interpretation, and request errors. Evidence is limited to a small, manually conducted invoice evaluation. This report is not a formal benchmark or certification and makes no claim of general performance.

## 13. Appendix: classifications and records

The definitions above are the methodology definitions; logged classifications are preserved without silent changes. T09 is PASS in the CSV despite its answer-type anomaly. T22's conservative-abstention rationale and the invalid premises for T23-T24 are described in Sections 3, 5, and 6.

Source files: [raw evaluation log](../results/evaluation-log.csv), [snapshot](../results/summary.md), [methodology](methodology.md), [thematic findings](findings.md), [Experiment 2 source and expected answers](../test-cases/experiment-02-custom-invoice.md), and [future evaluation plan](future-evaluation.md). The original preloaded invoice and complete Experiment 1 test-case record were not preserved. The repository has no explicit license; no license was selected for this evaluation. The CSV is the row-level results record; this report, findings, and summary are interpretive documentation.

