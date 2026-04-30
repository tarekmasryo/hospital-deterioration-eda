# Technical Notes — Early-Warning EDA Safeguards

## 1. Pre-event analysis view

For early-warning analysis, rows at or after the deterioration event are not valid warning windows.

The notebook creates:

```python
pre_event_panel = make_pre_event_view(panel)
```

This view keeps:

- all rows for patients without deterioration
- only rows before `deterioration_hour` for patients with deterioration

This avoids diluting the next-12h target with post-event rows that are no longer actionable warning windows.

---

## 2. Clustering feature policy

Early physiologic phenotypes are built from first-six-hour vitals and labs only.

Excluded from clustering:

- `patient_id`
- outcome columns
- deterioration timing columns
- simulator context variables
- baseline risk and comorbidity fields

This keeps clustering descriptive and avoids grouping patients by identifiers or target-adjacent fields.

---

## 3. Baseline risk score interpretation

`baseline_risk_score` is a synthetic simulator-generated latent signal.

It can be used as a reference signal for EDA and threshold intuition, but it should not be described as:

- a clinical score
- a deployable model output
- a validated risk model

The notebook therefore calls the threshold section a **synthetic reference alert policy view**.

---

## 4. Modeling boundary

This repository is an EDA notebook, not a model deployment project.

A production-style modeling notebook should separately include:

- patient-level train/validation/test separation
- leakage-safe preprocessing
- threshold selection on validation only
- final test reporting once
- calibration and alert-burden reporting
- monitoring assumptions and failure modes
