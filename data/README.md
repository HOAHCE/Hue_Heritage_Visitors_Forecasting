# Data

## Provenance

Daily visitor counts recorded at ticket gates by the **Hue Monuments Conservation Centre**
(Trung tâm Bảo tồn Di tích Cố đô Huế) for three sites of the Complex of Hue Monuments, a
UNESCO World Heritage property in Thua Thien Hue province, Viet Nam.

The counts are administrative ticketing records, not survey estimates. They were supplied
as site-level daily totals split by visitor origin (domestic / international).

## Coverage

- **Period:** 2022-11-01 → 2026-08-24
- **Frequency:** daily, no gaps
- **Sites:** 3
- **Rows:** 1,393 per site; 4,179 in total

| File | Site (Vietnamese) | Site (English) |
|---|---|---|
| `raw/hue_dai_noi_daily_visitors_clean.csv` | Đại Nội | Imperial City (Dai Noi) |
| `raw/hue_lang_minh_mang_daily_visitors_clean.csv` | Lăng vua Minh Mạng | Minh Mang Tomb |
| `raw/hue_lang_khai_dinh_daily_visitors_clean.csv` | Lăng vua Khải Định | Khai Dinh Tomb |

All three files share the same schema and the same calendar, so they can be concatenated
directly (this is what the notebook does).

## Data dictionary

| Column | Type | Description |
|---|---|---|
| `date` | date (`YYYY-MM-DD`) | Calendar day of the observation. Contiguous daily index with no gaps. |
| `destination_id` | integer | Internal site identifier from the source system. |
| `destination` | string | Site name in Vietnamese. Used as the grouping key (`group_ids`) throughout the analysis. |
| `domestic_visitors` | integer | Number of domestic visitors admitted that day. |
| `international_visitors` | integer | Number of international visitors admitted that day. |
| `visitors` | integer | **Modelling target.** `domestic_visitors + international_visitors`. |
| `log1p_visitors` | float | `log(1 + visitors)`. The scale on which models are fitted; all reported errors are converted back to the original count scale. |
| `split` | string | Chronological partition: `train`, `validation` or `test`. Stored in the file so the partition is fixed and reproducible. |
| `is_zero_day` | boolean | `True` where `visitors == 0` (e.g. full-day closure). |
| `data_quality_flag` | string | Record provenance. Every row in this release is `observed_complete`, i.e. directly observed with no imputation. |

> **Note on encoding.** The CSV files are UTF-8 with a byte-order mark (BOM) and contain
> Vietnamese diacritics in `destination`. Read them with `encoding="utf-8-sig"` (pandas:
> `pd.read_csv(path, encoding="utf-8-sig")`) to avoid a stray `﻿` in the first
> column name.

## Splits

Chronological, never shuffled, and identical across the three sites:

| Split | Dates | Days | Share |
|---|---|---|---|
| `train` | 2022-11-01 → 2025-07-02 | 975 | 70.0% |
| `validation` | 2025-07-03 → 2026-01-26 | 208 | 14.9% |
| `test` | 2026-01-27 → 2026-08-24 | 210 | 15.1% |

Model selection and TFT hyper-parameter search use `train` + `validation` only. All
reported accuracy is computed on `test` by rolling origin.

## Derived features (created in the notebook, not stored here)

The notebook constructs these from `date` at run time, so they are not duplicated in the
raw files:

- **Lags:** `lag_1, lag_2, lag_3, lag_4, lag_5, lag_6, lag_7, lag_14, lag_21, lag_28`
- **Calendar:** `weekday`, `month`, `day`, `dayofyear`, `weekofyear`, `year`, `is_weekend`
- **Holidays:** `holiday`, `days_to_holiday` — Vietnamese public holidays via the
  [`holidays`](https://pypi.org/project/holidays/) package

No exogenous covariates (weather, festivals, search interest) are used.

## Licence

Data are released under **CC BY 4.0** — see [`../LICENSE-DATA`](../LICENSE-DATA). Please
cite the associated article and this repository when reusing them.
