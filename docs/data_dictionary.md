# Hospital Deterioration Dataset — Data Dictionary

This dataset represents a **high-fidelity simulated hospital cohort** designed for
early-warning and deterioration modeling.

The records are generated via a rules-based and probabilistic simulation calibrated
to typical hospital patterns. They **do not correspond to real patients** and contain
no identifiable information.

- Time is expressed as **hours from admission**.
- Each patient stay is capped at **72 hours**.
- All identifiers (`patient_id`) are artificial and non-identifiable.
- All features are fully observed (no missing values).

---

## 1. `patients.csv` — Patient-level static data

**Granularity:** one row per patient (10,000 patients).

| column_name                               | type      | description                                                                                  | allowed_values / range                                   | missing_values |
|-------------------------------------------|-----------|----------------------------------------------------------------------------------------------|----------------------------------------------------------|----------------|
| `patient_id`                              | int       | Unique patient identifier                                                                    | 1–10,000                                                 | None           |
| `age`                                     | int       | Age at admission (years)                                                                     | 18–90                                                    | None           |
| `gender`                                  | category  | Biological sex                                                                               | `"M"`, `"F"`                                             | None           |
| `comorbidity_index`                       | int       | Aggregate comorbidity burden (higher = more chronic disease)                                 | 0–8                                                      | None           |
| `admission_type`                          | category  | Admission route to the hospital                                                              | `"ED"`, `"Elective"`, `"Transfer"`                       | None           |
| `baseline_risk_score`                     | float     | Latent **baseline deterioration risk** at admission on a 0–1 scale (simulation parameter, not a clinical score) | ~0.03–0.98                               | None           |
| `los_hours`                               | int       | Length of stay in hours (capped at 72 hours)                                                 | 12–72                                                    | None           |
| `deterioration_event`                     | int (0/1) | Indicator for any clinical deterioration event during the stay                               | 0 = no event, 1 = at least one event                    | None           |
| `deterioration_within_12h_from_admission` | int (0/1) | Deterioration event occurs within the first 12 hours from admission                          | 0, 1                                                     | None           |
| `deterioration_hour`                      | int       | Hour-from-admission of the **first** deterioration event; `-1` means no event during the stay | -1 (no event) or 0–(los_hours - 1) (up to 71 in this dataset) | None     |

---

## 2. `vitals_timeseries.csv` — Hourly vital signs

**Granularity:** one row per `(patient_id, hour_from_admission)` for vital signs.

| column_name           | type      | description                                                                 | allowed_values / range                                   | missing_values |
|-----------------------|-----------|-----------------------------------------------------------------------------|----------------------------------------------------------|----------------|
| `patient_id`          | int       | Patient identifier (matches `patients.csv`)                                 | 1–10,000                                                 | None           |
| `hour_from_admission` | int       | Hour index from admission (0 = admission hour)                              | 0–(los_hours - 1), up to 71 in this dataset             | None           |
| `heart_rate`          | float     | Heart rate at that hour (beats per minute)                                  | ~40–180                                                  | None           |
| `respiratory_rate`    | float     | Respiratory rate at that hour (breaths per minute)                          | ~8–45                                                    | None           |
| `spo2_pct`            | float     | Peripheral oxygen saturation percentage                                     | ~70–100                                                  | None           |
| `temperature_c`       | float     | Body temperature in degrees Celsius                                         | ~35.2–40.5                                               | None           |
| `systolic_bp`         | float     | Systolic blood pressure (mmHg)                                              | ~70–185                                                  | None           |
| `diastolic_bp`        | float     | Diastolic blood pressure (mmHg)                                             | ~40–110                                                  | None           |
| `oxygen_device`       | category  | Type of oxygen delivery device                                              | `"none"`, `"nasal"`, `"mask"`, `"hfnc"`, `"niv"`         | None           |
| `oxygen_flow`         | float     | Oxygen flow rate (liters per minute). Exactly `0.0` whenever `oxygen_device == "none"`, and strictly positive only when an oxygen device is in use. | 0–~60 (only >0 for `"nasal"`, `"mask"`, `"hfnc"`, `"niv"`) | None |
| `mobility_score`      | int       | Ordinal mobility score (higher = more independent)                          | 0–4 (ordinal scale)                                     | None           |
| `nurse_alert`         | int (0/1) | Whether a nurse alert was triggered in that hour                            | 0, 1                                                     | None           |

---

## 3. `labs_timeseries.csv` — Hourly lab results

**Granularity:** one row per `(patient_id, hour_from_admission)` for lab values.

| column_name           | type      | description                                                                 | allowed_values / range                       | missing_values |
|-----------------------|-----------|-----------------------------------------------------------------------------|----------------------------------------------|----------------|
| `patient_id`          | int       | Patient identifier (matches `patients.csv`)                                 | 1–10,000                                     | None           |
| `hour_from_admission` | int       | Hour index from admission (aligned with vital signs)                        | 0–(los_hours - 1), up to 71 in this dataset | None           |
| `wbc_count`           | float     | White blood cell count (arbitrary clinical units, simulation-based)         | ~2–30                                        | None           |
| `lactate`             | float     | Serum lactate level (approx. mmol/L)                                       | ~0.5–8.0                                     | None           |
| `creatinine`          | float     | Serum creatinine (approx. mg/dL)                                           | ~0.4–4.5                                     | None           |
| `crp_level`           | float     | C-reactive protein level (approx. mg/L)                                    | ~0–250                                       | None           |
| `hemoglobin`          | float     | Hemoglobin concentration (approx. g/dL)                                    | ~7–17                                        | None           |
| `sepsis_risk_score`   | float     | Latent **sepsis risk** score at that hour on a 0–1 scale (simulation parameter) | ~0.02–1.00                              | None           |

---

## 4. `hospital_deterioration_hourly_panel.csv` — Full joined hourly panel

**Granularity:** one row per `(patient_id, hour_from_admission)` combining:

- Hourly vital signs  
- Hourly lab values  
- Static patient features  
- All deterioration labels

This file is convenient for EDA and modeling where you want everything in one table.

| column_name                               | type      | description                                                                                               | allowed_values / range                                   | missing_values |
|-------------------------------------------|-----------|-----------------------------------------------------------------------------------------------------------|----------------------------------------------------------|----------------|
| `patient_id`                              | int       | Patient identifier                                                                                        | 1–10,000                                                 | None           |
| `hour_from_admission`                     | int       | Hour index from admission                                                                                 | 0–(los_hours - 1), up to 71 in this dataset             | None           |
| `heart_rate`                              | float     | Heart rate (beats per minute) at this hour                                                                | ~40–180                                                  | None           |
| `respiratory_rate`                        | float     | Respiratory rate (breaths per minute)                                                                     | ~8–45                                                    | None           |
| `spo2_pct`                                | float     | Peripheral oxygen saturation (%)                                                                          | ~70–100                                                  | None           |
| `temperature_c`                           | float     | Body temperature in °C                                                                                    | ~35.2–40.5                                               | None           |
| `systolic_bp`                             | float     | Systolic blood pressure (mmHg)                                                                            | ~70–185                                                  | None           |
| `diastolic_bp`                            | float     | Diastolic blood pressure (mmHg)                                                                           | ~40–110                                                  | None           |
| `oxygen_device`                           | category  | Oxygen delivery device                                                                                    | `"none"`, `"nasal"`, `"mask"`, `"hfnc"`, `"niv"`         | None           |
| `oxygen_flow`                             | float     | Oxygen flow rate (L/min). Exactly `0.0` whenever `oxygen_device == "none"`, and strictly positive only when an oxygen device is in use. | 0–~60 (only >0 for `"nasal"`, `"mask"`, `"hfnc"`, `"niv"`) | None |
| `mobility_score`                          | int       | Ordinal mobility score                                                                                    | 0–4                                                      | None           |
| `nurse_alert`                             | int (0/1) | Nurse alert triggered during this hour                                                                    | 0, 1                                                     | None           |
| `wbc_count`                               | float     | White blood cell count                                                                                    | ~2–30                                                    | None           |
| `lactate`                                 | float     | Serum lactate                                                                                             | ~0.5–8.0                                                 | None           |
| `creatinine`                              | float     | Serum creatinine                                                                                          | ~0.4–4.5                                                 | None           |
| `crp_level`                               | float     | C-reactive protein                                                                                        | ~0–250                                                   | None           |
| `hemoglobin`                              | float     | Hemoglobin                                                                                                | ~7–17                                                    | None           |
| `sepsis_risk_score`                       | float     | Latent sepsis risk score (0–1)                                                                            | ~0.02–1.00                                               | None           |
| `age`                                     | int       | Age at admission                                                                                          | 18–90                                                    | None           |
| `gender`                                  | category  | Biological sex                                                                                            | `"M"`, `"F"`                                             | None           |
| `comorbidity_index`                       | int       | Aggregate comorbidity burden                                                                              | 0–8                                                      | None           |
| `admission_type`                          | category  | Admission route                                                                                           | `"ED"`, `"Elective"`, `"Transfer"`                       | None           |
| `baseline_risk_score`                     | float     | Latent baseline deterioration risk at admission on a 0–1 scale (simulation parameter)                    | ~0.03–0.98                                               | None           |
| `los_hours`                               | int       | Length of stay in hours                                                                                   | 12–72                                                    | None           |
| `deterioration_event`                     | int (0/1) | Any deterioration event during the stay                                                                   | 0, 1                                                     | None           |
| `deterioration_within_12h_from_admission` | int (0/1) | Deterioration occurs within the first 12 hours                                                            | 0, 1                                                     | None           |
| `deterioration_hour`                      | int       | Hour of first deterioration event; `-1` = no event                                                        | -1 (no event) or 0–(los_hours - 1)                      | None           |
| `deterioration_next_12h`                  | int (0/1) | Label: deterioration occurs **after this hour** and **within the next 12 hours** (see definition below)  | 0, 1                                                     | None           |

**Definition of `deterioration_next_12h`:**

For a given row with `(patient_id, hour_from_admission = t)` and corresponding `deterioration_hour = h`:

- `deterioration_next_12h = 1` if `t < h ≤ t + 12`
- `deterioration_next_12h = 0` otherwise (including no event in the stay).

---

## 5. `hospital_deterioration_ml_ready.csv` — ML-ready classification panel

**Granularity:** one row per hourly observation (per patient and `hour_from_admission`).  
**Intended use:** feed directly into ML models for **next-12h deterioration prediction**.

This file keeps only **features + target**, and omits identifiers and auxiliary targets to reduce leakage risk.

| column_name               | type      | description                                                                 | allowed_values / range                                   | missing_values |
|---------------------------|-----------|-----------------------------------------------------------------------------|----------------------------------------------------------|----------------|
| `hour_from_admission`     | int       | Hour index from admission                                                  | 0–71 (per-stay capped length in this dataset)           | None           |
| `heart_rate`              | float     | Heart rate (beats per minute)                                              | ~40–180                                                  | None           |
| `respiratory_rate`        | float     | Respiratory rate (breaths per minute)                                      | ~8–45                                                    | None           |
| `spo2_pct`                | float     | Peripheral oxygen saturation (%)                                           | ~70–100                                                  | None           |
| `temperature_c`           | float     | Body temperature (°C)                                                      | ~35.2–40.5                                               | None           |
| `systolic_bp`             | float     | Systolic blood pressure (mmHg)                                             | ~70–185                                                  | None           |
| `diastolic_bp`            | float     | Diastolic blood pressure (mmHg)                                            | ~40–110                                                  | None           |
| `oxygen_device`           | category  | Oxygen delivery device                                                     | `"none"`, `"nasal"`, `"mask"`, `"hfnc"`, `"niv"`         | None           |
| `oxygen_flow`             | float     | Oxygen flow rate (L/min). Exactly `0.0` whenever `oxygen_device == "none"`, and strictly positive only when an oxygen device is in use. | 0–~60 (only >0 for `"nasal"`, `"mask"`, `"hfnc"`, `"niv"`) | None |
| `mobility_score`          | int       | Ordinal mobility score                                                     | 0–4                                                      | None           |
| `nurse_alert`             | int (0/1) | Nurse alert active in this hour                                            | 0, 1                                                     | None           |
| `wbc_count`               | float     | White blood cell count                                                     | ~2–30                                                    | None           |
| `lactate`                 | float     | Serum lactate                                                              | ~0.5–8.0                                                 | None           |
| `creatinine`              | float     | Serum creatinine                                                           | ~0.4–4.5                                                 | None           |
| `crp_level`               | float     | C-reactive protein                                                         | ~0–250                                                   | None           |
| `hemoglobin`              | float     | Hemoglobin                                                                 | ~7–17                                                    | None           |
| `sepsis_risk_score`       | float     | Latent sepsis risk score (0–1)                                            | ~0.02–1.00                                               | None           |
| `age`                     | int       | Age at admission                                                           | 18–90                                                    | None           |
| `gender`                  | category  | Biological sex                                                             | `"M"`, `"F"`                                             | None           |
| `comorbidity_index`       | int       | Aggregate comorbidity burden                                               | 0–8                                                      | None           |
| `admission_type`          | category  | Admission route                                                            | `"ED"`, `"Elective"`, `"Transfer"`                       | None           |
| `deterioration_next_12h`  | int (0/1) | Target label: deterioration occurs **after this hour** and **within the next 12 hours** | 0, 1                                         | None           |

The definition of `deterioration_next_12h` is identical to the one in `hospital_deterioration_hourly_panel.csv`.
