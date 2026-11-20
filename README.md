# XERJ Reader-1 Evaluation

An exploratory evaluation of XERJ Reader-1's document extraction and reasoning using invoice questions. The repository preserves the 25-slot result log, the custom synthetic invoice, and the analysis of observed behavior.

## Purpose and scope

The evaluation examined direct extraction, evidence grounding, financial calculation, date interpretation, refusal behavior, answer typing, and request errors. It used a preloaded invoice and a custom synthetic invoice. It is an exploratory case review, not a formal or statistically representative benchmark.

Questions were submitted manually through the XERJ Reader-1 Playground. Expected outputs were compared with the source document; evidence and formula output were inspected where available, and confidence or latency was recorded when provided.

## Results

| Recorded classification | Slots |
|---|---:|
| PASS | 14 |
| FAIL | 7 |
| INCONCLUSIVE | 4 |
| OBSERVATION | 0 |

These are verified counts from the [raw evaluation log](results/evaluation-log.csv), not an accuracy percentage. Key observations include evidence-grounded extractions, an incorrect invoice total, false refusals for information reported as present, a date-role interpretation error, and two internal-error slots. T09 returned the correct shipping amount but was labeled Text and remains logged as PASS. T23-T24 had invalid absence premises. T22 remains PASS under a documented conservative-abstention rationale, although the requested contextual explanation was not returned.

## Documentation

- [Final evaluation report](docs/final-evaluation-report.md) - detailed results, failure analysis, limitations, and recommendations
- [Methodology](docs/methodology.md)
- [Thematic findings](docs/findings.md)
- [Results snapshot](results/summary.md)
- [Custom invoice test case and expected answers](test-cases/experiment-02-custom-invoice.md)
- [Future evaluation plan](docs/future-evaluation.md) - proposed work, separate from the completed evaluation

## Repository structure

```text
.
|-- README.md
|-- .gitignore
|-- docs/
|   |-- final-evaluation-report.md
|   |-- findings.md
|   |-- future-evaluation.md
|   `-- methodology.md
|-- results/
|   |-- evaluation-log.csv
|   `-- summary.md
`-- test-cases/
    `-- experiment-02-custom-invoice.md
```

## Limitations

Testing was manual and limited to invoice documents. The custom invoice is synthetic. The original preloaded invoice and complete original test-case record were not preserved, so Experiment 1 cannot be fully reproduced from this repository. No general accuracy, reliability, or cross-document performance claim is made. The repository has no explicit license.

## Product

[XERJ Reader-1](https://platform.xerj.ai/)
