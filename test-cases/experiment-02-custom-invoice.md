# Experiment 2 — Synthetic Invoice

## Source Document

```text
INVOICE

Invoice Number: INV-TEST-002
Invoice Date: 19 November 2026
Due Date: 19 December 2026

Seller: Apex Industrial Supplies
Buyer: Orion Data Systems

ITEMS

Industrial Sensor A
Quantity: 15
Unit Price: EUR 240.00
Line Amount: EUR 3,600.00

Industrial Sensor B
Quantity: 8
Unit Price: EUR 1,250.00
Line Amount: EUR 10,000.00

Connector Module C
Quantity: 4
Unit Price: EUR 850.00
Line Amount: EUR 3,400.00

Subtotal: EUR 17,000.00

Discount: 10% of subtotal
Discount Amount: EUR 1,700.00

Taxable Amount After Discount: EUR 15,300.00

VAT: 18% of taxable amount
VAT Amount: EUR 2,754.00

Shipping Charge: EUR 450.00
Shipping is added after VAT calculation.

TOTAL PAYABLE: To be calculated.

Payment Terms: 30 days.
```

## Independently Calculated Ground Truth

| Component | Value |
|---|---:|
| Subtotal | EUR 17,000.00 |
| Discount | EUR 1,700.00 |
| Taxable amount | EUR 15,300.00 |
| VAT | EUR 2,754.00 |
| Shipping | EUR 450.00 |
| Total payable | EUR 18,504.00 |

Calculation:

17,000 - 1,700 + 2,754 + 450 = 18,504

## Tests

| ID | Question | Expected Answer |
|---|---|---|
| T06 | What is the total payable amount on this invoice, including the discount, VAT and shipping charge? | EUR 18,504.00 |
| T07 | What is the discount amount applied to this invoice? | EUR 1,700.00 |
| T08 | What is the VAT amount stated on this invoice? | EUR 2,754.00 |
| T09 | What is the shipping charge on this invoice? | EUR 450.00 |
| T10 | Quote the exact line in the invoice that states the discount amount. | Discount Amount: EUR 1,700.00 |
| T11 | What is the invoice number? | INV-TEST-002 |
| T12 | Who is the seller on this invoice? | Apex Industrial Supplies |
| T13 | What is the due date on this invoice? | 19 December 2026 |

## Evaluation Notes

Expected answers were determined independently from the synthetic source document.

The tests are exploratory. They are not designed to establish statistical performance.
