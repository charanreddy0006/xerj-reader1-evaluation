# Findings

This file groups observations from the evaluation log by theme. The row-level record remains [results/evaluation-log.csv](../results/evaluation-log.csv); analysis is in the [final evaluation report](final-evaluation-report.md).

## Direct extraction and evidence

- T01-T05 in Experiment 1 were recorded as meeting their expected outcomes, including an expected refusal for a CEO reported absent.
- T09 returned the expected EUR 450 shipping charge, but the answer type was Text.
- T11 returned `INV-TEST-002` with an exact source span.
- T14 and T16-T18 recorded successful invoice-number, IBAN, email, and seller-address extraction from the preloaded invoice, with evidence spans noted.
- T20 derived 2026-11-30 from the invoice date and 30-day payment terms, with evidence for both inputs.
- T25 returned EUR as the invoice currency with an exact evidence span.

These are individual observations from a small sample, not a general accuracy claim.

## False refusals on information reported present

| ID | Question recorded | Expected | Actual | Confidence | Latency |
|---|---|---|---|---:|---:|
| T07 | What is the discount amount applied to this invoice? | EUR 1,700.00 | DECLINE ABSENT | 0.984 | 211 ms |
| T08 | What is the VAT amount stated on this invoice? | EUR 2,754.00 | DECLINE ABSENT | 0.956 | 204 ms |
| T10 | Quote the exact line in the invoice that states the discount amount. | Discount Amount: EUR 1,700.00 | DECLINE ABSENT | 0.995 | 208 ms |
| T15 | BIC (CSV question field; full prompt not retained) | COBADEFFXXX | DECLINE ABSENT | 0.98 | 254 ms |
| T19 | Seller name buyer name and invoice number (CSV question field) | All three fields extracted | DECLINE ABSENT | 0.98 | 229 ms |

These are false refusals in the recorded document and scenarios, not an estimate of refusal behavior across documents. Confidence is system-reported and not independently calibrated.

## Calculation and contextual interpretation

- T06 returned EUR 18,530 instead of EUR 18,504. The custom invoice states a EUR 17,000 subtotal, EUR 1,700 discount, EUR 2,754 VAT, and EUR 450 shipping added after VAT. The expected calculation is `17,000 - 1,700 + 2,754 + 450 = 18,504`. The recorded finding describes the generated formula as mathematically valid but semantically inconsistent with the invoice instructions.
- T21 answered that dates conflicted when the invoice date and due date served distinct roles. It did not explain the role of each date as requested. This is one observation; the underlying cause is not established.

## Abstention and invalid test premises

- T22 remains classified PASS in the CSV. The documented rationale is that abstaining on the purchase-order purpose was conservative because the document did not explicitly explain that purpose. The requested contextual explanation was not returned, so this PASS should not be read as successful contextual interpretation.
- T23 was intended to test refusal for absent bank details, but the invoice contained an account number at characters 829-856; Reader-1 extracted it with evidence. The premise was invalid, so the result remains INCONCLUSIVE and does not establish hallucination resistance.
- T24 was intended to test refusal for an absent VAT registration number, but the invoice contained `DE 812 455 921` at characters 82-96; Reader-1 extracted it with evidence. The premise was invalid, so the result remains INCONCLUSIVE and does not establish hallucination resistance.

## Service errors

- T12 returned internal errors on two attempts when asked for the seller. The preserved request ID is `req_fecaw4tt68vps3gg31gq4xht`.
- T13 returned an internal error when asked for the due date. The preserved request ID is `req_7e56dmqy33vx62q78xxvfxrf`.

Both are INCONCLUSIVE, not model-accuracy failures. Available records do not identify the cause.

## Confidence and latency

Confidence values are system-reported and have not been independently calibrated. For the false refusals in T07, T08, T10, T15, and T19, reported confidence ranged from 0.956 to 0.995. Latency is recorded for 17 of 25 slots, ranging from 186 to 286 ms; eight slots have no recorded latency. These are sample observations, not system-wide performance measures.

## Limitations

The findings come from 25 planned slots, two invoice contexts, and manual exploratory testing. The original preloaded invoice and complete Experiment 1 test-case record were not preserved. These observations do not establish general accuracy, reliability, calibration, or performance on other document types.

