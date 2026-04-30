# Case Study — Hospital Deterioration Early-Warning EDA

## Problem

Hospital deterioration work is not only a modeling problem. Before building any prediction pipeline, teams need to understand:

- which patient segments deteriorate more often
- when deterioration tends to happen
- how vitals and labs move before events
- how much alert burden a threshold policy may create
- where analysis can accidentally include post-event information

This project uses a synthetic hospital cohort to build a careful, decision-ready EDA workflow for early-warning analysis.

---

## Dataset

The repository expects five CSV files:

- `patients.csv`
- `vitals_timeseries.csv`
- `labs_timeseries.csv`
- `hospital_deterioration_hourly_panel.csv`
- `hospital_deterioration_ml_ready.csv`

The data is synthetic. Patient identifiers are artificial, and `baseline_risk_score` is a simulator-generated latent signal rather than a validated clinical score.

---

## Methodology

The notebook is organized around four questions.

### 1. Is the dataset structurally reliable?

The workflow checks:

- row and column counts
- missing values
- duplicate patient-hour keys
- patient ID coverage across files
- basic physiologic ranges
- consistency between patient-level and hourly-panel labels

### 2. Who deteriorates?

The EDA reviews deterioration prevalence across:

- age bands
- admission types
- comorbidity bands
- length-of-stay bands
- baseline risk deciles

### 3. When does deterioration happen?

The notebook analyzes deterioration timing across the admission window and highlights that late-hour views represent patients still under observation, not the original full cohort.

### 4. What changes before deterioration?

The analysis compares event-centered vitals and population-level trajectories for key vitals and labs, then derives early physiologic phenotypes using only first-six-hour vitals and labs.

---

## Important safeguards

This version includes two critical corrections for early-warning analysis:

1. **Pre-event view:** post-event and event-time rows are removed from next-12h alert-policy analysis.
2. **Clustering guardrail:** identifiers, outcomes, and simulator context fields are excluded from clustering features.

These safeguards keep the notebook aligned with the early-warning setting and avoid misleading exploratory results.

---

## Outputs

The notebook produces:

- cohort KPI table
- data-quality summary
- event funnel
- subgroup deterioration views
- event-timing charts
- vitals/labs trajectory plots
- early phenotype clusters
- pre-event next-12h risk views
- baseline risk × hour heatmap
- synthetic alert-threshold curve

---

## Decision value

This notebook is useful as a first-stage analysis before:

- a leak-safe next-12h deterioration modeling notebook
- an alert simulator
- a dashboard for operational review
- a model-monitoring or data-quality workflow

It does not claim deployment readiness. Its role is to make the data, risks, and operating trade-offs visible before modeling.

---

## Limitations

- Synthetic data only.
- No real clinical validation.
- No deployment-ready prediction model.
- Threshold views are exploratory and must not be treated as clinical policy.
