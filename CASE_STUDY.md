# Case Study — Hospital Deterioration EDA (Early Warning Context)

## Overview
This repository provides a clinically-minded EDA workflow for understanding **deterioration risk** during the early portion of an admission.

The goal is to translate cohort tables into operator-friendly insights:
who deteriorates, when deterioration happens, and what physiologic patterns tend to precede events.

## The real problem
Early warning work breaks down when analysis is not connected to operations:
- teams track aggregate outcomes but miss high-risk subgroups
- deterioration timing is unclear, so alerts come too late or too often
- alert fatigue rises because thresholds are not aligned with staffing capacity

EDA is the first step in making these trade-offs explicit.

## What this notebook answers
- **Cohort shape:** demographics, admission types, comorbidity burden
- **Event prevalence:** how frequent deterioration is, and for whom
- **Event timing:** when events concentrate within the first 72 hours
- **Risk signal sanity:** does `baseline_risk_score` correlate with outcomes (if provided)?
- **Physiology before events:** how vitals/labs behave leading up to deterioration vs stable patients

## Outputs that matter
Typical outputs to capture for reporting:
- event prevalence by subgroup (age band, comorbidity band, admission type)
- time-to-event distributions and peaks
- vitals/labs trajectory comparisons (event vs no-event)
- sanity checks on any baseline risk score

These outputs are designed to feed into:
- a next-step modeling baseline (12h prediction)
- alert simulator assumptions (expected alert volume)
- stakeholder discussions on operating thresholds

## Limitations
- This is an exploratory workflow; it does not produce a deployment-ready model.
- Cohort and logging assumptions can vary across sites; conclusions should be validated locally.
- Clinical deployment requires governance, monitoring, and regulatory review where applicable.
