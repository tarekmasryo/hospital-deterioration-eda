# 🏥 Hospital Deterioration — Clinical EDA Notebook

Practical, clinically-minded EDA for a **simulated hospital deterioration cohort** over the first 72 hours of admission.  
The notebook turns raw cohort tables into **early-warning intuition**: who deteriorates, when, and under which physiologic patterns.

---

## 📌 Goals

This notebook focuses on:

- Describing **who is in the cohort** and how often deterioration happens.
- Profiling risk by **age, comorbidity index, admission type, and length of stay**.
- Showing **when** deterioration events occur in the first 72 hours.
- Checking how a latent **`baseline_risk_score`** behaves as a risk signal.
- Exploring **vitals and labs trajectories** before events vs no-event patients.
- Building simple **early physiologic clusters** from early vitals + labs.
- Looking at the **next-12h label**, **risk “hot zones”**, and a toy **alert policy**.

---

## 🗃️ Dataset & Files

The notebook assumes five core CSVs in the working directory (or `DATA_DIR`):

- `patients.csv`  
  One row per admission: demographics, comorbidity index, admission route,  
  length of stay, deterioration outcomes, baseline risk score.

- `vitals_timeseries.csv`  
  Hourly vitals per patient: heart rate, respiratory rate, SpO₂, temperature,  
  blood pressure, and hour from admission.

- `labs_timeseries.csv`  
  Hourly labs: lactate, WBC, creatinine, CRP, hemoglobin, etc.

- `hospital_deterioration_hourly_panel.csv`  
  Joined hourly panel including risk score, next-12h label, and context.

- `hospital_deterioration_ml_ready.csv`  
  Compact, ML-ready table for **next-12h deterioration** prediction.

You can point `DATA_DIR` to a different path if running on Kaggle or another environment.

---

## 🔍 What this notebook covers

1. **Cohort structure & outcomes**  
   - KPI snapshot (patients, mean age, LOS, event rates).  
   - Core distributions: age, gender, admissions, comorbidities, LOS.  
   - Event funnel from *all patients → any deterioration → early deterioration*.

2. **Stratified deterioration risk**  
   - Event rates by **age bands**, **comorbidity bands**, and **admission type**.  
   - LOS × comorbidity **heatmap** to highlight higher-risk segments.

3. **Timeline of events**  
   - Distribution of **deterioration hour** from admission.  
   - Event shares across time bands (0–12h, 13–24h, 25–48h, 49–72h).

4. **Baseline risk score vs observed outcomes**  
   - Deciles of `baseline_risk_score` vs any / early deterioration.  
   - Quick calibration-style view to see if the score behaves like a usable risk signal.

5. **Physiologic trajectories around events**  
   - Event-centred curves (−24h → 0h) for heart rate, respiratory rate, SpO₂, temperature.  
   - Population-level trajectories (0–24h) for event vs no-event patients, for both vitals and labs.

6. **Early physiologic phenotypes (clusters)**  
   - Aggregated 0–6h vitals + labs per patient.  
   - **KMeans (k=3)** clusters as early physiologic phenotypes.  
   - Cluster-level event rate, baseline risk, and comorbidity.  
   - 2D **PCA map** coloured by cluster and deterioration outcome.

7. **Next-12h label & risk hot zones**  
   - Mean `deterioration_next_12h` by hour from admission.  
   - Correlation of numeric ML features with the next-12h label.  
   - Feature distributions split by next-12h outcome.

8. **Alert policy view**  
   - Hour × risk-decile heatmap for `deterioration_next_12h`.  
   - Simple alert policy using `baseline_risk_score` thresholds:  
     precision/recall vs alerts per 100 windows.

9. **Patient storyboards**  
   - Side-by-side stories for one deteriorating and one non-deteriorating patient  
     using their vitals and lactate trajectories over time.

---

## 🧪 How to run

### Requirements

- Python 3.10+
- `pandas`, `numpy`
- `matplotlib`, `seaborn`
- `scikit-learn`

Install with:

```bash
pip install -r requirements.txt
# or
pip install pandas numpy matplotlib seaborn scikit-learn
```

### Run the notebook

```bash
git clone https://github.com/<tarekmasryo>/hospital-deterioration-eda.git
cd hospital-deterioration-eda

# Place the CSV files under DATA_DIR (default: current folder)

jupyter notebook "Hospital Deterioration — Clinical EDA Notebook.ipynb"
```

Update `DATA_DIR` at the top of the notebook if your CSVs live elsewhere.

---

## 🚀 Suggested follow-up work

- **Next-12h Early Warning Baseline**  
  Train baseline classifiers on `hospital_deterioration_ml_ready.csv`  
  (logistic regression, tree-based models, calibration, cost-aware thresholds).

- **Sequence models & policy optimisation**  
  Work with time-series architectures (RNN/GRU/Temporal CNN/transformer-style),  
  add better calibration, and explore threshold/paging policies under different  
  alert budgets and clinical cost assumptions.
