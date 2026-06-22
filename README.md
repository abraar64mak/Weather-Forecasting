# 🌦️ Global Weather Trend Forecasting

> **PM Accelerator Tech Assessment — Data Scientist / Analyst (Advanced Track)**

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python)](https://www.python.org/)
[![Platform](https://img.shields.io/badge/Platform-Google%20Colab-orange?logo=googlecolab)](https://colab.research.google.com/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)
[![Dataset](https://img.shields.io/badge/Dataset-Kaggle-20BEFF?logo=kaggle)](https://www.kaggle.com/datasets/nelgiriyewithana/global-weather-repository)

---

## 📌 PM Accelerator Mission

> The Product Manager Accelerator Program is designed to support PM professionals through every stage of their careers. From students looking for their first Product Management job to Directors looking to take on a leadership role, our program has helped over hundreds of students fulfill their career aspirations.
>
> Our Product Manager Accelerator community is ambitious and committed. Through our program, students are able to gain knowledge in the Product field, sharpen their skills, and ultimately, land their dream jobs. Our program offers true hands-on experience through the collaboration with industry leading companies. Our strategies are tailored to each individual student's needs, propelling them to successfully break into PM roles or move forward in their existing PM career.

*Source: [pmaccelerator.io](https://www.pmaccelerator.io) — founded by Dr. Nancy Li.*

---

## 📖 Project Overview

This project analyzes the **Global Weather Repository** dataset — 148,320 daily weather snapshots across **268 cities in 211 countries**, spanning May 2024 to June 2026 — to forecast weather trends, detect anomalies, and uncover climate and environmental patterns.

The analysis follows the **Advanced Track** requirements and covers:

| # | Task | Method |
|---|------|--------|
| 1 | Data Cleaning & Preprocessing | Duplicate removal, physical outlier detection, datetime parsing |
| 2 | Exploratory Data Analysis | Distributions, correlations, global trends, city deep-dive |
| 3 | Anomaly Detection | Rolling Z-Score + Isolation Forest |
| 4 | Time Series Forecasting | Seasonal Naive, SARIMA+Fourier, Prophet, XGBoost |
| 5 | Ensemble Model | Simple average + Inverse-RMSE weighted ensemble |
| 6 | Climate Analysis | Temperature & precipitation by latitude band |
| 7 | Environmental Impact | Air quality vs. weather correlations |
| 8 | Feature Importance | Random Forest — Gini + Permutation importance |
| 9 | Spatial Analysis | Geographic temperature scatter map |
| 10 | Geographical Patterns | Continent-level temperature & PM2.5 comparison |

---

## 📁 Repository Structure

```
weather-forecast/
│
├── notebooks/
│   └── weather_forecasting.ipynb     # Main notebook (all analysis & models)
│
├── outputs/                          # Auto-generated chart PNGs (created at runtime)
│   ├── eda_distributions.png
│   ├── correlation_heatmap.png
│   ├── global_trends.png
│   ├── tokyo_temp_precip.png
│   ├── anomalies_statistical.png
│   ├── anomalies_isolation_forest.png
│   ├── forecast_comparison.png
│   ├── ensemble_forecast.png
│   ├── climate_by_latitude.png
│   ├── air_quality_correlation.png
│   ├── top_pm25_countries.png
│   ├── feature_importance.png
│   ├── spatial_temperature_map.png
│   └── continent_comparison.png
│
├── data/                             # Not committed — download instructions below
│   └── GlobalWeatherRepository.csv
│
├── requirements.txt
├── .gitignore
└── README.md
```

---

## 🗂️ Dataset

**Source:** [Global Weather Repository — Kaggle](https://www.kaggle.com/datasets/nelgiriyewithana/global-weather-repository)

| Attribute | Value |
|-----------|-------|
| Rows | 148,320 |
| Columns | 41 |
| Countries | 211 |
| Cities | 268 |
| Date range | May 2024 – June 2026 |
| Missing values | 0 |

The dataset includes meteorological variables (temperature, wind, pressure, humidity, precipitation, UV index), air quality measurements (PM2.5, PM10, CO, O₃, NO₂, SO₂, US-EPA and UK-DEFRA indices), astronomical data (sunrise, sunset, moon phase), and geospatial identifiers.

> **Note:** The CSV is ~38 MB and Kaggle-licensed, so it is not committed to this repository. Download it from the link above and place it at `data/GlobalWeatherRepository.csv` before running locally, or upload it directly to `/content/` if using Google Colab.

---

## 🚀 How to Run

### Option A — Google Colab (Recommended)

1. Open the notebook in Colab:

   [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

2. Upload `GlobalWeatherRepository.csv` to Colab's file panel (it will land at `/content/GlobalWeatherRepository.csv`).

3. Run all cells top to bottom — the notebook auto-creates the `outputs/` folder and saves all charts there.

> All required libraries (Prophet, XGBoost, statsmodels, scikit-learn) are available on Colab without any manual installation.

---

### Option B — Local (Jupyter)

**1. Clone the repository**

```bash
git clone https://github.com/<your-username>/weather-forecast.git
cd weather-forecast
```

**2. Install dependencies**

```bash
pip install -r requirements.txt
```

**3. Download the dataset**

Download `GlobalWeatherRepository.csv` from [Kaggle](https://www.kaggle.com/datasets/nelgiriyewithana/global-weather-repository) and place it at:

```
weather-forecast/data/GlobalWeatherRepository.csv
```

**4. Launch the notebook**

```bash
jupyter notebook notebooks/weather_forecasting.ipynb
```

Run all cells. Charts are saved automatically to `outputs/`.

---

## 📦 Dependencies

```
pandas>=2.0
numpy>=1.24
matplotlib>=3.7
seaborn>=0.12
scikit-learn>=1.3
statsmodels>=0.14
prophet>=1.1
xgboost>=2.0
jupyter>=1.0
ipykernel>=6.0
```

Install all at once:

```bash
pip install -r requirements.txt
```

---

## 📊 Key Results

### Forecasting — 30-Day Tokyo Temperature Holdout

| Model | MAE (°C) | RMSE (°C) | MAPE (%) |
|-------|----------|-----------|----------|
| Seasonal Naive (baseline) | ~5.2 | ~6.3 | ~20.1 |
| XGBoost | ~4.4 | ~5.6 | ~17.0 |
| SARIMA + Fourier | ~3.9 | ~4.9 | ~15.2 |
| Ensemble (Weighted) | ~3.8 | ~5.0 | ~14.8 |
| **Prophet** | **~3.6** | **~4.6** | **~14.0** |

All three trained models beat the seasonal-naive baseline by ~25–30% in RMSE. The inverse-RMSE weighted ensemble is competitive with the best single model while providing lower variance across different forecast windows.

### Anomaly Detection

| Method | Scope | Anomalies Found |
|--------|-------|-----------------|
| Rolling Z-Score (&#124;z&#124; > 2.0) | Tokyo (univariate) | 17 days |
| Isolation Forest (contamination=1%) | Global (multivariate) | ~1,483 records |

### Top Insight per Section

- **EDA:** Precipitation is zero-inflated and right-skewed; air quality pollutants co-move (shared emission sources).
- **Climate:** Temperature variance *increases* with latitude — tropical cities are warm and stable; polar cities swing 20–30°C seasonally.
- **Air Quality:** Wind speed is the strongest weather-side predictor of pollution, consistently negative across all pollutants (ventilation effect).
- **Feature Importance:** Latitude dominates all importance rankings — the single strongest physical determinant of baseline temperature globally.
- **Spatial:** The latitude-temperature gradient is visible even without a basemap; hottest cities cluster in the Middle East and Sub-Saharan Africa.

---

## 🔬 Methodology Notes

**Why Tokyo for forecasting?**
All 268 cities have identical data density (~763 records each), so the choice was driven by interpretability. Tokyo has a textbook temperate seasonal cycle, is globally recognisable, and has enough data (>2 full annual cycles) for proper seasonal model validation.

**Why Fourier terms instead of SARIMA(p,d,q)(P,D,Q)[365]?**
A seasonal order of 365 in `statsmodels` SARIMAX is computationally intractable — the state-space matrix scales with the seasonal period, making it take hours to fit. Fourier exogenous regressors (4 sin/cos pairs) capture the smooth annual cycle in seconds with equivalent forecast quality.

**Why two anomaly methods?**
Rolling Z-Score is univariate and city-specific — good for catching sudden spikes relative to local recent history. Isolation Forest is multivariate and global — good for catching readings that are jointly unusual across several variables even when no single variable appears extreme. Using both provides mutual validation.

---

## 📋 Submission Checklist

- [x] Data cleaning & preprocessing
- [x] EDA with visualizations
- [x] Anomaly detection (statistical + model-based)
- [x] Multiple forecasting models (3 models)
- [x] Model evaluation — MAE, RMSE, MAPE
- [x] Ensemble model (average + weighted)
- [x] Climate analysis
- [x] Environmental / air quality impact
- [x] Feature importance (Gini + Permutation)
- [x] Spatial analysis
- [x] Geographical patterns
- [x] PM Accelerator mission displayed
- [x] GitHub repository with README
- [x] `requirements.txt`
- [ ] Demo video — *link to be added*

---

## 🎥 Demo Video

> 📹 [[Watch the demo here](https://youtu.be/WfWV6DFJ9NA)](#) ← *replace with your video URL before submitting*

The demo walks through the notebook in ~2 minutes, covering data cleaning, EDA highlights, anomaly detection, model comparison, and the unique analyses.

---

## 👤 Author

**Mohammed Abraar Khan**
M.S. in Artificial Intelligence Systems — University of Florida (May 2026)

[![Email](https://img.shields.io/badge/Email-abraarkhan64@gmail.com-red?logo=gmail)](mailto:abraarkhan64@gmail.com)
[![GitHub](https://img.shields.io/badge/GitHub-abraar64mak-black?logo=github)](https://github.com/abraar64mak)

---

## 📄 License

This project is open-source under the [MIT License](LICENSE).
The dataset is subject to Kaggle's terms of use — see the [dataset page](https://www.kaggle.com/datasets/nelgiriyewithana/global-weather-repository) for details.
