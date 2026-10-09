# Research-on-the-reliability-of-loan-applicants

# Price Analysis of Apartment Sales in the Leningrad Region

![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-data%20analysis-150458?logo=pandas&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-statistics-F7931E?logo=scikitlearn&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-hypothesis%20testing-8CAAE6?logo=scipy&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)

An exploratory data analysis project on apartment listings in Saint Petersburg and the surrounding Leningrad region. The workflow covers data cleaning, feature engineering, exploratory visualisation, correlation analysis, statistical hypothesis testing (Z-test and t-test), and outlier detection using the IQR method.

---

## Table of Contents

- [Overview](#overview)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Key Results](#key-results)
- [Statistical Tests](#statistical-tests)
- [Outlier Analysis](#outlier-analysis)
- [Practical Recommendations](#practical-recommendations)
- [Limitations](#limitations)
- [Author](#author)

---

## Overview

The real estate market is driven by location, size, layout, and building characteristics. This project answers a focused question: **what factors are most strongly associated with apartment prices in Saint Petersburg and the Leningrad region, and can observed differences be confirmed statistically?**

**Objectives**

1. Load and clean 23,699 apartment listings
2. Handle missing values and remove outliers
3. Engineer new features (price per m², distance to centre, floor type, ceiling category)
4. Explore price drivers with histograms, boxplots, violin plots, and heatmaps
5. Test two hypotheses: open-plan share by building height, and price difference between Saint Petersburg and other localities
6. Identify anomalous listings using the IQR method

## Dataset

| Property | Value |
|---|---|
| File | `real_estate_data.csv` |
| Original size | 23,699 rows × 22 columns |
| Cleaned size | ~20,394 rows |
| Period | 2015 – 2019 |
| Target | `last_price` — final listing price (RUB) |
| Key features | `total_area`, `living_area`, `kitchen_area`, `rooms`, `ceiling_height`, `floor`, `floors_total`, `locality_name`, `cityCenters_nearest`, `days_exposition` |

**Why these variables?**

- `total_area` and `living_area` capture the physical size of the property
- `ceiling_height` reflects building quality and the luxury segment
- `locality_name` and `cityCenters_nearest` capture location value
- `rooms`, `floor`, `floors_total` describe layout and building type

## Methodology

**1. Data loading and initial preprocessing**
Load the CSV, inspect structure with `head()`, `info()`, and `describe()`. Check for duplicate rows and remove them.

**2. Missing value handling**

| Column | Missing % | Strategy |
|---|---|---|
| `last_price`, `total_area`, `rooms`, `first_day_exposition`, `locality_name` | 0% | No action |
| `floors_total` | 0.36% | Drop rows (< 5%) |
| `locality_name` | 0.21% | Drop rows (< 5%) |
| `ceiling_height` | 38.8% | Fill with median |
| `balcony` | 48.6% | Fill with 0 (no balcony) |
| `cityCenters_nearest` | 23.3% | Fill with median |

After cleaning: 3,305 rows removed (13.95% of the original dataset).

**3. Feature engineering**

- `year`, `month`, `weekday`, `day` — extracted from `first_day_exposition`
- `price_per_sqm` — `last_price / total_area`
- `distance_km` — `cityCenters_nearest / 1000`
- `floor_type` — "ground/first floor", "top floor", or "other"
- `ceiling_category` — "low" (< 2.5 m), "standard" (2.5–2.7 m), "high" (> 2.7 m)

**4. Outlier removal**

Rows with `total_area` < 10 or > 200 m², `last_price` < 1e6 or > 50e6 RUB, and `ceiling_height` < 2.2 or > 3.5 m were removed to keep the analysis focused on the typical market segment.

**5. Exploratory data analysis**

Histograms, boxplots, violin plots, pivot tables, and a correlation heatmap were used to identify the main price drivers and seasonal patterns.

**6. Statistical hypothesis testing**

- **Z-test** for proportions (`proportions_ztest`): open-plan share in high-rise vs low-rise buildings
- **Shapiro–Wilk** for normality, **Levene** for variance equality
- **Two-sample t-test**: price per m² in Saint Petersburg vs other localities

## Key Results

### Model quality on the cleaned dataset

| Metric | Value |
|---|---|
| Final row count | ~20,394 |
| Rows removed | 3,305 (13.95%) |
| Mean `days_exposition` | 178.7 days |
| Median `days_exposition` | 94 days |

The mean is nearly twice the median, showing a right-skewed distribution — some listings stayed online for a very long time.

### Main price drivers (correlation with `last_price`)

| Feature | Correlation |
|---|---|
| `total_area` | **0.70** (strongest) |
| `kitchen_area` | 0.58 |
| `rooms` | 0.47 |
| `ceiling_height` | 0.39 |

### Top 5 localities by number of listings

| Locality | Listings | Mean price per m² (RUB) |
|---|---|---|
| Saint Petersburg | 13,123 | 109,534 |
| Murino | 513 | 85,712 |
| Shushary | 406 | 77,847 |
| Vsevolozhsk | 335 | 68,516 |
| Kolpino | 307 | 74,763 |

**Key insight:** Saint Petersburg dominates both listing volume and average price per m², at roughly **30% above** the surrounding localities.

### Price vs distance from the city centre

Price per square metre drops sharply within the first 5 km — from approximately **RUB 119,000/m²** at the centre to **RUB 40,000–50,000/m²** at 5 km. Beyond 5 km, the price stabilises and declines more slowly. After 40 km, it rises slightly, likely due to suburban and dacha demand.

### Seasonality

- **Listing activity:** peaks in March–April (spring) and October–November (autumn); lowest in June–August and January.
- **Price per m²:** highest in September–October (autumn); lower in summer and early in the year.
- **Interpretation:** supply rises in spring, but prices respond with a delay and increase in autumn when supply falls and demand remains.

### Ceiling height and price

| Category | Median price per m² | Spread |
|---|---|---|
| High (> 2.7 m) | Highest median, many high-price outliers | Widest |
| Standard (2.5–2.7 m) | Median ≈ RUB 100,000/m² | Moderate |
| Low (< 2.5 m) | Median ≈ RUB 80,000/m² | Narrowest |

Higher ceilings are associated with a higher price per square metre and greater price variation.

## Statistical Tests

### Test 4.1 — Open-plan share by building height (Z-test)

- **H₀:** the proportions are equal
- **H₁:** the proportion is higher in high-rise buildings (≥ 10 floors)

| Metric | Value |
|---|---|
| p-value | 6.81 × 10⁻⁸ |
| Decision | Reject H₀ |
| Conclusion | The proportion of open-plan apartments is **statistically significantly higher** in high-rise buildings than in low-rise buildings (≤ 5 floors). |

### Test 4.2 — Price per m² in Saint Petersburg vs other localities (t-test)

- **H₀:** the average prices are equal
- **H₁:** the average price in Saint Petersburg is higher

| Test | Result | Conclusion |
|---|---|---|
| Shapiro–Wilk (SPb) | p ≈ 9.0 × 10⁻⁸⁴ | Non-normal |
| Shapiro–Wilk (others) | p ≈ 3.2 × 10⁻⁵⁰ | Non-normal |
| Levene (variance) | p ≈ 6.0 × 10⁻¹⁷ | Unequal variances |
| Two-sample t-test | **p ≈ 0.0** | Reject H₀ |

**Boxplot comparison:**

- Saint Petersburg median ≈ RUB 100,000/m²
- Other localities median ≈ RUB 70,000/m²
- Wider interquartile range in Saint Petersburg — greater price variation

**Conclusion:** price per square metre in Saint Petersburg is considerably and statistically significantly higher than in other localities.

### Full correlation matrix

| Pair | Correlation | Interpretation |
|---|---|---|
| `total_area` ↔ `living_area` | 0.92 | Nearly proportional |
| `rooms` ↔ `living_area` | 0.88 | More rooms → larger living area |
| `total_area` ↔ `rooms` | 0.79 | More rooms → larger total area |
| `last_price` ↔ `total_area` | 0.77 | Area is the strongest price driver |
| `price_per_sqm` ↔ `last_price` | 0.72 | Strong |
| `price_per_sqm` ↔ `total_area` | 0.20 | Weak — price per m² depends more on location and quality |

## Outlier Analysis

Using the IQR method on `price_per_sqm`:

- **Q1** = 76,923 RUB/m²
- **Q3** = 111,697 RUB/m²
- **IQR** = 34,774 RUB/m²
- **Upper bound** = 163,857 RUB/m²
- **Anomalous apartments:** 720 (≈ 3.5% of cleaned data)

**Characteristics of anomalous apartments:**

- Average price per m²: ≈ RUB 193,000; maximum: RUB 640,422/m²
- Average area: ≈ 83 m² — high price is not explained by area alone
- Median distance to centre: ≈ 6 km — relatively close to the centre
- Median ceiling height: 2.7 m; many have ceilings of 3.0–3.43 m
- Median rooms: 2 — medium-sized apartments

**Possible explanations:**

- Premium locations close to the city centre
- High ceilings associated with the luxury segment
- Listings in luxury residential complexes with premium finishes

## Practical Recommendations

1. **Use `total_area` as a core pricing variable** — it has the strongest correlation with price (r = 0.70).

2. **Apply a location adjustment.** Price per square metre drops sharply within the first 5 km from the centre. A linear distance adjustment underestimates this effect — a piecewise or spline term would fit better.

3. **Treat Saint Petersburg as a separate segment.** Price per square metre is roughly 30% higher than in nearby localities, so a single valuation model for the whole region may not be appropriate.

4. **Summer may be a favourable buying window.** Prices per square metre are lower in June–August while supply remains available, giving buyers more negotiating room than in autumn.

5. **Apply an upward adjustment for high ceilings.** Apartments with ceilings above 2.8 m consistently appear in the high-price segment, even after controlling for area.

6. **Flag IQR outliers as a separate segment.** The 720 anomalous listings should be evaluated with a different model rather than the general one, since the general model would systematically under-predict them.

## Limitations

- **Coverage gap.** Data for 2015 and 2019 covers only six months, so yearly averages for those years are not directly comparable to full years.
- **Non-normal distributions.** Shapiro–Wilk rejects normality for both price-per-m² groups, and Levene rejects equal variances — the t-test is formally approximate, though the sample is large enough for the Central Limit Theorem to apply.
- **Missing `ceiling_height` in 38.8% of rows.** Filling with the median reduces variance and may weaken the apparent relationship between ceiling height and price.
- **Geographic scope.** Results are specific to Saint Petersburg and the Leningrad region and may not generalise to other markets.

**Possible next steps:** build a regression model on log-price, apply gradient boosting (XGBoost/LightGBM) for price prediction, add macroeconomic variables (interest rates, construction volumes), and split the model by locality to capture local price dynamics.

## Author

**Nguyen Dinh Trieu**
Economics (Analytical Economics and Econometrics), Plekhanov Russian University of Economics
[trieu31072004@gmail.com](mailto:trieu31072004@gmail.com)
