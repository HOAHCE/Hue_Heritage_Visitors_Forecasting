# Code

## Contents

| File | Description |
|---|---|
| `Visitors_Forecasting_RF_TFT_perHorizon_v5.ipynb` | Complete analysis pipeline: data loading and feature construction, all eight models, multi-seed benchmarking, statistical testing, probabilistic evaluation, SHAP interpretation, cost profiling and figure generation. |

The notebook is self-contained: running it end to end regenerates every file in
`results/`. It is organised in ten sections that map onto the manuscript.

## Notebook structure

| Section | Contents |
|---|---|
| 1 | Environment setup and `CONFIG` |
| 2 | Data loading, regularisation, calendar and Vietnamese holiday features |
| 3 | Error metrics and the Diebold-Mariano test |
| 4 | Benchmark models (SeasonalNaive-7, ETS, SARIMAX, ARIMAX, RF, LSTM, DLM) + rolling-origin evaluation + cost timing |
| 5 | Temporal Fusion Transformer: Optuna search, multi-seed training, quantile forecasts |
| 5.2 | Probabilistic evaluation of TFT quantiles (pinball loss, PICP, MPIW) |
| 6 | Aggregation to mean ± 95% CI, per-horizon model selection, Diebold-Mariano, Model Confidence Set, figures, rank stability and the RF ↔ TFT crossover check |
| 7 | SHAP interpretation of the RandomForest |
| 8 | Computational-cost summary |
| 9 | Management and policy implications |
| 10 | Limitations and future work |

> The narrative markdown cells are written in Vietnamese; **all generated figure titles,
> axis labels and exported table columns are in English** (with English site names:
> Imperial City / Minh Mang Tomb / Khai Dinh Tomb) so that outputs drop straight into the
> manuscript. Figure filenames use ASCII slugs: `dai_noi`, `minh_mang`, `khai_dinh`.

## Running it

### Configuration

Edit `CONFIG` in Section 1. The two paths must be changed from the original Google Colab
locations:

```python
CONFIG = {
    "DATA_DIR":   "data/raw",     # originally a Google Drive path
    "OUTPUT_DIR": "results",      # originally a Google Drive path
    "USE_LOG_TARGET": True,
    "MAX_ENCODER_LENGTH": 30,
    "HORIZONS": [1, 7, 30],
    "SEED": 42,
    "SEEDS": [42, 7, 123, 2024, 1, 99, 256, 512, 77, 2025],
    "TFT_TUNE": True,
    "TUNE_TRIALS": 10,
    "TUNE_EPOCHS": 8,
    "MAX_EPOCHS": 30,
    "BATCH_SIZE": 128,
    "DESTINATIONS": ["Đại Nội", "Lăng vua Minh Mạng", "Lăng vua Khải Định"],
    "MODELS": ["SeasonalNaive7", "ETS", "SARIMAX", "ARIMAX", "RF", "LSTM", "DLM", "TFT"],
    "TFT_QUANTILES": [0.02, 0.1, 0.25, 0.5, 0.75, 0.9, 0.98],
    "MCS_SIZE": 0.10,
    "MCS_REPS": 1000,
}
```

`DESTINATIONS` must match the `destination` values in the CSV files exactly, including
Vietnamese diacritics.

### Locally

```bash
pip install -r ../requirements.txt
jupyter lab Visitors_Forecasting_RF_TFT_perHorizon_v5.ipynb
```

Delete or skip the `google.colab.drive.mount(...)` cell in Section 1 — it is Colab-only.

### In Google Colab

Upload the repository (or mount your Drive), select a **GPU** runtime, set `DATA_DIR` and
`OUTPUT_DIR`, and run all cells. The first cell installs the dependencies.

### Quick smoke test

Set `QUICK_MODE = True` in Section 1 to run with fewer seeds and epochs before committing
to the full run.

## Runtime

The TFT dominates. Mean fit time per run, measured on the hardware used for the published
results (`../results/tables/compute_cost.csv`):

| Model | Mean fit (s) | Runs |
|---|---|---|
| SeasonalNaive7 | ~0.000002 | 3 |
| DLM | 0.12 | 30 |
| ETS | 0.31 | 3 |
| LSTM | 0.38 | 30 |
| RF | 0.98 | 30 |
| SARIMAX | 1.89 | 3 |
| ARIMAX | 2.09 | 3 |
| **TFT** | **45.25** | 30 |

A full run is 3 sites × 10 seeds plus the Optuna search, so budget several GPU-hours.
Deterministic models (SeasonalNaive-7, ETS, SARIMAX, ARIMAX) are fitted once and
replicated across seeds, which is why their run counts are 3 rather than 30.

## Licence

Code is released under the **MIT** licence — see [`../LICENSE`](../LICENSE).
