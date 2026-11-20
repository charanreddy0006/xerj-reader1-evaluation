# Evaluation Snapshot

**Product:** XERJ Reader-1  
**Evaluation:** Exploratory document extraction evaluation  
**Scope:** 25 planned test slots across two invoice experiments  
**Evaluation date:** Not recorded in the source files

## Recorded classifications

Recalculated from the 25 data rows in `evaluation-log.csv`:

| Experiment | Slots | PASS | FAIL | INCONCLUSIVE | OBSERVATION |
|---|---:|---:|---:|---:|---:|
| Experiment 1: preloaded invoice | 17 | 12 | 3 | 2 | 0 |
| Experiment 2: custom synthetic invoice | 8 | 2 | 4 | 2 | 0 |
| **Total** | **25** | **14** | **7** | **4** | **0** |

These are classification counts, not accuracy estimates. T12 returned internal errors on two attempts; T13 returned an internal error. T23 and T24 are inconclusive because their target values were present, contrary to the test premise. T22 remains PASS per the conservative-abstention rationale, although the requested contextual explanation was not returned; see the [full report](../docs/final-evaluation-report.md#classification-criteria).

## Observations

- Direct extraction and exact evidence spans were observed in selected cases.
- False refusals were recorded for several fields documented as present.
- T06 returned EUR 18,530 instead of the independently calculated EUR 18,504.
- T20 correctly derived a due date from invoice date plus payment terms; T21 misinterpreted different date roles as a conflict.
- T09 returned the right shipping value but labeled it Text.
- T09 remains classified PASS in the CSV, although its type anomaly also fits the general OBSERVATION definition; the report documents this inconsistency without changing the log.
- Two slots returned internal errors.

See the [evaluation report](../docs/final-evaluation-report.md) and [raw CSV](evaluation-log.csv). This evaluation is exploratory and does not establish general performance.

