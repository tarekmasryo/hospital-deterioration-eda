# data/raw

Place the dataset CSV files here (not committed to the repository).

Required files:
- `patients.csv`
- `vitals_timeseries.csv`
- `labs_timeseries.csv`
- `hospital_deterioration_hourly_panel.csv`
- `hospital_deterioration_ml_ready.csv`

The notebook will use these local files if present, otherwise it falls back to the Kaggle input path.
