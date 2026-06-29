# ml-dashboard
How to Build an ML Monitoring Dashboard from Scratch (Streamlit Tutorial)

# ML Monitoring Dashboard

Companion code for **"How to Build an ML Monitoring Dashboard from Scratch
(Streamlit Tutorial)"** — Article 10 in the Production ML Engineering series.

Builds directly on the three monitor layers from Article 09 (FeatureMonitor,
PredictionMonitor, PerformanceMonitor) and wraps them in:

- a SQLite logging store for predictions and delayed ground truth
- a multi-page Streamlit dashboard (drift scores, confusion matrix, calibration)
- a Slack/email alert dispatcher with severity gating and rate limiting
- a scaling benchmark for write throughput and query latency

## Setup

```bash
pip install -r requirements.txt
```

## Run a scenario through the pipeline

```bash
python -m benchmarks.run_pipeline --scenario abrupt --db data/monitoring.db
```

Scenario options: `stable`, `abrupt`, `gradual`, `label_shift` — these
replicate the four scenarios from Article 09.

## Launch the dashboard

```bash
streamlit run dashboard/app.py
```

Point the sidebar "Database path" field at the `.db` file you generated
above. Three pages: overview (this file), Drift Scores, and Calibration.

## Run the test suite

```bash
python -m pytest tests/ -v
```

21 tests covering the logging store, all three monitors, the stream
generators, and alert dispatch.

## Run the scaling benchmark

```bash
python -m benchmarks.scaling_benchmark
```

## Project structure

```
detectors/        KS, PSI, MMD, ADWIN, DDM — ported from Article 09
monitors/          FeatureMonitor, PredictionMonitor, PerformanceMonitor
data/              stream generators + SQLite logging store
alerts/            Slack/email dispatch with severity gating
benchmarks/        pipeline runner + scaling benchmark
dashboard/         Streamlit app (app.py + pages/)
tests/             pytest suite
```

## License

MIT
