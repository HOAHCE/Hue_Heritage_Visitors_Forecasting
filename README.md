# Hue Heritage Visitors Forecasting

**Horizon-specific model selection for daily visitor demand at the Complex of Hue Monuments, Viet Nam**

Data, code and results supporting the manuscript

---

## Authors

**Quan Truong Tan** <sup>1</sup>, **Thi Ngoc Trang Tran** <sup>2</sup>, **Hoa Tran Thai** <sup>1,\*</sup>

<sup>1</sup> University of Economics - Hue University, 99 Ho Dac Di Street, Hue City, Vietnam
<sup>2</sup> Institute of Software Engineering and Artificial Intelligence, Graz University of Technology, Graz, Austria

<sup>\*</sup> Corresponding author

| Author | Affiliation | ORCID | Email |
|---|---|---|---|
| Quan Truong Tan | University of Economics - Hue University, 99 Ho Dac Di Street, Hue City, Vietnam | [0009-0001-0241-5512](https://orcid.org/0009-0001-0241-5512) | ttquan@hueuni.edu.vn |
| Thi Ngoc Trang Tran | Institute of Software Engineering and Artificial Intelligence, Graz University of Technology, Graz, Austria | — | trang.tran@tugraz.at |
| Hoa Tran Thai (corresponding author) | University of Economics - Hue University, 99 Ho Dac Di Street, Hue City, Vietnam | [0009-0006-2405-4697](https://orcid.org/0009-0006-2405-4697) | tranthaihoa@hueuni.edu.vn |

---

## Summary

Tourism-demand studies usually report a single "best" forecasting model. This study
shows that at daily resolution **no single model wins at every forecast horizon**, and
that the choice of model should follow the decision horizon it serves.

Using 1,393 consecutive days of visitor counts (2022-11-01 to 2026-08-24) at three
UNESCO World Heritage sites of the Complex of Hue Monuments, we benchmark eight
forecasting models at **1-, 7- and 30-day** horizons, repeated over **10 random seeds**,
and evaluate them with point-accuracy metrics, pairwise Diebold-Mariano tests, the
Model Confidence Set, probabilistic scoring of Temporal Fusion Transformer (TFT)
quantiles, SHAP-based interpretation, and computational-cost profiling.

**Headline finding — a reproducible RF → TFT crossover.** RandomForest is the most
accurate model for next-day forecasting at all three sites, while the TFT is the most
accurate at 7 and 30 days. The crossover holds in 10/10 seeds at the Imperial City and
Khai Dinh Tomb, and 9/10 seeds at Minh Mang Tomb.

### Best model per horizon (mean MAE across 10 seeds, 95% CI)

| Site | h = 1 day | h = 7 days | h = 30 days |
|---|---|---|---|
| Imperial City (Đại Nội) | **RF** 578.6 [575, 582] | **TFT** 882.0 [831, 933] | **TFT** 957.6 [835, 1081] |
| Minh Mang Tomb (Lăng vua Minh Mạng) | **RF** 134.7 [134, 135] | **TFT** 164.5 [151, 178] | **TFT** 177.3 [148, 207] |
| Khai Dinh Tomb (Lăng vua Khải Định) | **RF** 244.3 [243, 245] | **TFT** 324.5 [296, 353] | **TFT** 352.2 [315, 390] |

Full numbers, runner-up models and CI separation flags: `results/tables/best_per_horizon.csv`.

---

## Repository structure

```
.
├── data/
│   ├── raw/                                   # 3 site-level daily series (input to the notebook)
│   │   ├── hue_dai_noi_daily_visitors_clean.csv
│   │   ├── hue_lang_minh_mang_daily_visitors_clean.csv
│   │   └── hue_lang_khai_dinh_daily_visitors_clean.csv
│   └── README.md                              # data dictionary and provenance
├── code/
│   ├── Visitors_Forecasting_RF_TFT_perHorizon_v5.ipynb   # complete analysis pipeline
│   └── README.md                              # how to run, runtime, configuration
├── results/
│   ├── tables/                                # 12 result tables (CSV)
│   ├── figures/                               # 22 figures (PNG)
│   ├── predictions/                           # per-origin forecasts
│   └── README.md                              # inventory and column definitions
├── requirements.txt
├── CITATION.cff
├── AUTHORS.md
├── LICENSE                                    # MIT — code
└── LICENSE-DATA                               # CC BY 4.0 — data and results
```

---

## Data

Daily visitor counts at three heritage sites managed by the Hue Monuments Conservation
Centre, covering **2022-11-01 to 2026-08-24** (1,393 days per site, 4,179 site-days in
total, no missing days and no imputed values — every record carries
`data_quality_flag = observed_complete`).

| Site (Vietnamese) | Site (English) | `destination_id` | Days |
|---|---|---|---|
| Đại Nội | Imperial City (Dai Noi) | 5 | 1,393 |
| Lăng vua Minh Mạng | Minh Mang Tomb | — | 1,393 |
| Lăng vua Khải Định | Khai Dinh Tomb | — | 1,393 |

**Chronological splits** (identical across sites, no shuffling — the split column is
stored in the data so the partition is fixed and reproducible):

| Split | Dates | Days |
|---|---|---|
| train | 2022-11-01 → 2025-07-02 | 975 |
| validation | 2025-07-03 → 2026-01-26 | 208 |
| test | 2026-01-27 → 2026-08-24 | 210 |

See [`data/README.md`](data/README.md) for the full data dictionary.

---

## Methods

**Target.** `visitors = domestic_visitors + international_visitors`, modelled on the
`log1p` scale and evaluated back on the original count scale.

**Horizons.** 1, 7 and 30 days ahead, evaluated by rolling origin over the test block.

**Models (8).**

| Family | Models |
|---|---|
| Baseline | SeasonalNaive-7 |
| Statistical | ETS, SARIMAX, ARIMAX (calendar + holiday exogenous variables) |
| Machine learning | RandomForest (lag + calendar feature matrix) |
| Deep learning | LSTM, DLM, Temporal Fusion Transformer (TFT) |

**Features.** Lags {1,2,3,4,5,6,7,14,21,28} plus calendar variables (weekday, month,
day, day-of-year, weekend indicator) and Vietnamese public-holiday variables (holiday
indicator, days-to-holiday). No weather, event or search-interest covariates are used —
these are deliberately left to future work.

**Protocol.** Statistical models are fitted once on train+validation and *applied* to
the expanding history at each origin; ML/DL models forecast recursively. This keeps the
comparison fair across families. The whole benchmark is repeated over **10 seeds**
(42, 7, 123, 2024, 1, 99, 256, 512, 77, 2025); deterministic models are run once and
replicated across seeds.

**Evaluation.**
- Point accuracy: MAE, RMSE, MSE, MAPE, R², reported as mean ± 95% CI across seeds.
- **Diebold-Mariano** tests of the per-horizon champion against every other model.
- **Model Confidence Set** (Hansen, Lunde & Nason, 2011), 90% confidence, 1,000 bootstrap
  replications, block size `max(h, 10)`, on per-origin absolute errors.
- **Probabilistic**: pinball loss over TFT quantiles {0.02, 0.1, 0.25, 0.5, 0.75, 0.9, 0.98},
  with PICP and MPIW at nominal 80% and 95%.
- **Interpretation**: SHAP `TreeExplainer` on the RandomForest.
- **Cost**: wall-clock fit and inference time per model.

---

## Reproducing the analysis

### 1. Environment

```bash
git clone https://github.com/HOAHCE/Hue_Heritage_Visitors_Forecasting.git
cd Hue_Heritage_Visitors_Forecasting

python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

Python 3.10+ is recommended. A CUDA-capable GPU is strongly recommended: the full run is
3 sites × 10 seeds and the TFT dominates the runtime (mean fit ≈ 45 s per run versus
< 3 s for every other model — see `results/tables/compute_cost.csv`).

### 2. Point the notebook at the data

Open `code/Visitors_Forecasting_RF_TFT_perHorizon_v5.ipynb` and edit the `CONFIG`
dictionary in **Section 1**:

```python
CONFIG = {
    "DATA_DIR":   "data/raw",        # was a Google Drive path
    "OUTPUT_DIR": "results",         # was a Google Drive path
    ...
}
```

The notebook was developed in Google Colab, so it contains a `drive.mount(...)` cell —
skip or delete that cell when running locally.

### 3. Run

Run all cells top to bottom. Set `QUICK_MODE = True` in Section 1 for a fast smoke test
(fewer seeds and epochs) before committing to the full run.

Outputs are written to `results/tables/`, `results/figures/` and `results/predictions/`,
overwriting the files shipped here.

### Determinism

Seeds are fixed and the chronological splits are stored in the data, so tables and
figures reproduce up to the usual non-determinism of GPU floating-point reductions in
PyTorch. Statistical baselines and the RandomForest reproduce exactly.

---

## Key results at a glance

| Artefact | File |
|---|---|
| Best model per horizon + CI separation | `results/tables/best_per_horizon.csv` |
| RF ↔ TFT crossover, seeds supporting it | `results/tables/crossover_check.csv` |
| Mean ± 95% CI for MAE / RMSE / MAPE | `results/tables/metrics_agg.csv` |
| Raw per-seed metrics | `results/tables/metrics_by_seed.csv` |
| Diebold-Mariano tests | `results/tables/dm_best_per_horizon.csv` |
| Model Confidence Set membership | `results/tables/mcs_per_horizon.csv` |
| TFT probabilistic calibration | `results/tables/tft_quantile_eval.csv` |
| SHAP feature importance (per site) | `results/tables/shap_importance_*.csv` |
| Computational cost | `results/tables/compute_cost.csv` |

**What drives short-horizon demand.** SHAP attributes most of the RandomForest signal to
`lag_1`, the weekly rhythm (`lag_7`, `lag_6`, `weekday`) and holiday proximity — cheap,
fast signals that a tree ensemble captures well, which is why RF suffices for day-ahead
operations.

**A caveat we report openly.** TFT prediction intervals are **narrower than nominal**:
empirical coverage is 51-70% against a nominal 80%, and 81-96% against a nominal 95%
(`results/tables/tft_quantile_eval.csv`). Conformal calibration (e.g. CQR) is identified
in the manuscript as the natural remedy and is left to future work.

---

## Management implications

| Horizon | Model | Operational / policy decision |
|---|---|---|
| 1 day (operational) | RandomForest | Daily staffing, ticket-counter and gate opening, crowd control, guide allocation |
| 7 days (tactical) | TFT | Weekly rostering, F&B and souvenir stock, tour-operator coordination, maintenance on low-demand days |
| 30 days (strategic) | TFT | Monthly revenue forecasting and budgeting, marketing timing, restoration scheduling, capacity and tiered-ticket management |

This supports a **two-tier deployment**: a lightweight RF dashboard on site for daily
operations, and a periodic provincial-level TFT run for budgeting, marketing and
conservation planning.

---

## Limitations

- Three comparable sites only; transfer to smaller or structurally different sites is untested.
- No exogenous covariates (weather, festivals such as Festival Huế, Google Trends) — demand-shock forecasting is left to future work.
- TFT intervals are under-calibrated (see above).
- Probabilistic forecasts are produced for the TFT only, so the probabilistic comparison is not like-for-like across all eight models.

---

## Licence

- **Code** (`code/`): [MIT](LICENSE)
- **Data and results** (`data/`, `results/`): [CC BY 4.0](LICENSE-DATA)

## Citation

Please cite both the article and this repository. Machine-readable metadata is in
[`CITATION.cff`](CITATION.cff).

```bibtex
@software{truongtan_hue_heritage_forecasting_2026,
  author  = {Truong Tan, Quan and Tran, Thi Ngoc Trang and Tran Thai, Hoa},
  title   = {Hue Heritage Visitors Forecasting: horizon-specific model selection
             for daily visitor demand at Hue Imperial Citadel heritage sites},
  year    = {2026},
  version = {1.0.0},
  url     = {https://github.com/HOAHCE/Hue_Heritage_Visitors_Forecasting},
  license = {MIT}
}
```

## Acknowledgements

We thank the Hue Monuments Conservation Centre (Trung tâm Bảo tồn Di tích Cố đô Huế) for
the visitor-count records underlying this study.

## Contact

Hoa Tran Thai — tranthaihoa@hueuni.edu.vn (corresponding author)
