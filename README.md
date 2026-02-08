# 🏥 Hospital Deterioration — Clinical EDA Notebook

Clinically-minded EDA for a hospital deterioration cohort. The notebook turns raw cohort tables into **early-warning insights**:
who deteriorates, when, and under which physiologic patterns.

Notebook: `hospital-deterioration-eda.ipynb`  
Case study: `CASE_STUDY.md`

---

## What this repository includes
- Cohort overview (age, comorbidities, admission types)
- Event timing over the first 72 hours
- Baseline risk signal analysis (`baseline_risk_score` if present)
- Vitals and labs trajectories (event vs no-event)

---

## Dataset
Place the CSV files under `data/raw/`:

Required:
- `patients.csv`
- `vitals_timeseries.csv`
- `labs_timeseries.csv`
- `hospital_deterioration_hourly_panel.csv`
- `hospital_deterioration_ml_ready.csv`

See `data/raw/README.md`.

If running on Kaggle and local files are not present, the notebook falls back to Kaggle input paths.

---

## Quick start
```bash
python -m venv .venv
# Windows: .\.venv\Scripts\activate
# macOS/Linux: source .venv/bin/activate

pip install -r requirements.txt
```

Run the notebook:
- `hospital-deterioration-eda.ipynb`

---

## Disclaimer
This project is intended for educational and research use. It is **not** a certified medical device and must not be used for clinical decision-making without appropriate validation, governance, and regulatory review.
