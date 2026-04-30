# 🏥 Hospital Deterioration — Early Warning EDA

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)](#)
[![Notebook](https://img.shields.io/badge/Format-Jupyter%20Notebook-orange)](#)
[![Kaggle](https://img.shields.io/badge/Kaggle-Notebook-20BEFF)](https://www.kaggle.com/code/tarekmasryo/hospital-deterioration-eda)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

Clinically minded exploratory analysis for a **synthetic hospital deterioration dataset**.

The notebook turns patient-level, vitals, labs, and hourly-panel tables into a practical early-warning EDA workflow:
**who deteriorates, when it happens, which signals move before events, and how alert thresholds affect operational burden**.

> This repository is educational and research-oriented. It is not a medical device and must not be used for clinical decision-making.

---

## 📌 What this project covers

- 🧾 Dataset structure, missingness, duplicate keys, and patient coverage checks
- 👥 Cohort profile by age, admission type, comorbidity burden, and length of stay
- 🚨 Deterioration prevalence and event-timing patterns
- 📈 Vitals and labs trajectories before deterioration
- 🧬 Early physiologic phenotypes using MiniBatchKMeans
- 🔍 Pre-event next-12h label analysis for early-warning exploration
- 🌡️ Baseline-risk heatmap across admission hours
- 📊 Synthetic alert-threshold view showing recall, precision, and alert burden

---

## 🧠 Why this notebook is careful

Early-warning analysis is easy to get wrong. This notebook includes explicit safeguards:

- ✅ Uses a **pre-event panel** for next-12h warning analysis
- ✅ Removes post-event/event-time rows from alert-policy exploration
- ✅ Excludes `patient_id`, outcomes, and simulator context fields from clustering
- ✅ Treats `baseline_risk_score` as a **synthetic simulator-generated reference signal**, not a clinical score
- ✅ Keeps EDA separate from deployment-ready modeling claims

---

## 📂 Repository structure

```text
.
├── hospital-deterioration-eda.ipynb
├── README.md
├── CASE_STUDY.md
├── CHANGELOG.md
├── LICENSE
├── requirements.txt
├── data/
│   └── raw/
│       └── README.md
├── docs/
│   ├── data_dictionary.md
│   └── technical_notes.md
└── artifacts/
    └── README.md
```

---

## 📦 Dataset files

Place the dataset CSV files under `data/raw/` for local runs:

```text
patients.csv
vitals_timeseries.csv
labs_timeseries.csv
hospital_deterioration_hourly_panel.csv
hospital_deterioration_ml_ready.csv
```

On Kaggle, attach the dataset to the notebook. The notebook searches common Kaggle input paths and local paths automatically.

For schema details, see:

- [`docs/data_dictionary.md`](docs/data_dictionary.md)

---

## 🚀 Quick start

```bash
python -m venv .venv

# Windows
.\.venv\Scripts\activate

# macOS/Linux
source .venv/bin/activate

pip install -r requirements.txt
```

Then open and run:

```text
hospital-deterioration-eda.ipynb
```

---

## ✅ Main outputs

The notebook produces:

- cohort KPI summary
- data-quality and key-integrity tables
- event funnel
- subgroup risk views
- time-to-deterioration plots
- vitals/labs trajectory charts
- early physiologic clusters
- pre-event next-12h risk curve
- feature-vs-target distribution checks
- baseline-risk × hour heatmap
- synthetic alert curve

---

## ⚠️ Limitations

- The dataset is synthetic and does not represent real patients.
- The EDA does not create a deployment-ready clinical model.
- `baseline_risk_score` is a simulator-generated reference signal, not a validated medical score.
- Any real clinical application would require site-specific validation, governance, monitoring, and regulatory review.

---

## 👤 Author

**Tarek Masryo**

---

## 📄 License

MIT License. See [`LICENSE`](LICENSE) for details.
