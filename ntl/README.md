# Nighttime Lights (NTL) Datasets for Indonesia

This folder contains processed Nighttime Lights (NTL) datasets for Indonesia at the district level. Two main datasets are currently available.

---

## Dataset 1: Monthly VIIRS-like District Panel (1992–2024)

**File:** `viirs-like_monthly_district_panel_1992_2024.csv`

### Source
Cheng et al. (2026). A temporally consistent global 500 m-resolution monthly VIIRS-like nighttime light dataset (1992–2024). *Earth System Science Data*.

### Column Descriptions

#### Identification Columns
| Column | Description |
|--------|-------------|
| `district_code` | Unique identifier in the format `IDN_XXXX` |
| `districtID` | 4-digit district code based on **BPS (Badan Pusat Statistik)** classification. This column is the **primary key** for joining with other datasets |
| `district` | Official name of the district or city |
| `provinceID` | 2-digit province code based on BPS classification |
| `province` | Official province name |
| `islandID` | Island/regional grouping code (based on BPS classification) |
| `island` | Major island name |
| `month_year` | Observation period in `YYYY-MM` format |


#### Basic Statistics (Raw Aggregated Values)
| Column | Description |
|--------|-------------|
| `mean` | Average NTL radiance per pixel in the district |
| `median` | Median NTL radiance (robust to outliers) |
| `sd` | Standard deviation of radiance |
| `sum` | Total sum of radiance across all pixels |
| `count` | Number of valid pixels aggregated |
| `min` / `max` | Minimum and maximum radiance values |

#### Log-transformed & Seasonally Adjusted
| Column | Description |
|--------|-------------|
| `log_sum` | Natural log of total radiance (`log(sum)`) |
| `log_sum_sa_common` | Seasonally adjusted log of total radiance |
| `log_sum_sa_regime` | Seasonally adjusted with regime-specific method |

#### Temporal Filters & Smoothing

**Hodrick-Prescott (HP) Trend Filter**  
Decomposes the time series into trend and cyclical components. Higher λ produces smoother long-term trends.
- `hp_trend_lambda_14400`, `hp_trend_lambda_129600`, `hp_trend_lambda_1000000`
- Columns with `_trimmed` reduce edge effects at the start and end of the series.

**Kalman Filter (Local Level Model)**  
State-space model that estimates the underlying level while handling noise.
- Different `qr` values control how responsive the level is to changes (`qr_0p001`, `qr_0p01`, `qr_0p05`).
- `_trimmed` versions apply trimming for robustness.

**Rolling Median**
- `rolling_median_7m`, `rolling_median_13m`, `rolling_median_25m` (window size in months)

**Epanechnikov Kernel Smoothing**
- Columns follow the pattern `epan_rm{window}_k{bandwidth}` (e.g., `epan_rm7m_k5m`)
- Combines rolling median with kernel weighting for flexible smoothing.

**Validity Flags**
Boolean columns ending with `_valid` (e.g., `hp_valid_lambda_14400`, `kalman_valid_qr_0p01`).  
`TRUE` = the filtered value is considered reliable for analysis.

---

## Dataset 2: Annual District-Level NTL (LACC-based, 1992–2022)

**File:** `global_nighttime_light_dataset_district_1992_2022.csv`

### Source
Tang, H., Zhong, Y., Deng, J., Xia, H., & Wei, J. (2025). Global nighttime light dataset from 1992 to 2022 with focus on low-light areas. *Scientific Data*.

### Column Descriptions

| Column | Description |
|--------|-------------|
| `ID_ADMIN` | Unique administrative identifier (same as `district_code`) |
| `districtID` | 4-digit district code based on **BPS (Badan Pusat Statistik)** classification. This column is the **primary key** for joining with other datasets |
| `district` | Official name of the district or city |
| `provinceID` | 2-digit province code based on BPS classification|
| `province` | Province name |
| `islandID` | Island/regional grouping code (based on BPS classification) |
| `island` | Island name |
| `COORD_X` | Longitude (centroid of the district) |
| `COORD_Y` | Latitude (centroid of the district) |
| `year` | Year of observation (1992–2022) |
| `product` | Data source/product used (e.g., `LACC`) |
| `count` | Number of pixels aggregated in the district |
| `mean` | Average NTL radiance (DMSP-like scale 0–63) |
| `median` | Median NTL radiance |
| `sum` | Total sum of radiance (Total Sum of Lights) |
| `min` | Minimum radiance value |
| `max` | Maximum radiance value |
| `std` | Standard deviation of radiance |
| `filename` | Source raster file used for aggregation |

### Notes on Dataset 2
- Uses DMSP-like digital number scale (0–63).
- Particularly strong for low-light and rural areas.
- Annual frequency only.

---

## Citation

**Dataset 1 (Monthly VIIRS-like Panel):**
Cheng, H., Geng, M., Li, X., Li, S., Zhao, M., Lin, C., Wang, J., Gong, P., & Zhou, Y. (2026). A temporally consistent global 500 m-resolution monthly VIIRS-like nighttime light dataset (1992–2024). *Earth System Science Data*, 18, 3449–3479. https://doi.org/10.5194/essd-18-3449-2026

**Dataset 2 (Annual LACC-based):**
Tang, H., Zhong, Y., Deng, J., Xia, H., & Wei, J. (2025). Global nighttime light dataset from 1992 to 2022 with focus on low-light areas. *Scientific Data*, 12, 982. https://doi.org/10.1038/s41597-025-05246-8

---

*Last updated: June 2026*