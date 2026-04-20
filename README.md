# Air Pollution Forecasting — Delhi PM2.5 Time Series Analysis

Predicting daily fine particulate matter (PM2.5) concentrations in Delhi using statistical and deep learning models, with a full end-to-end data science pipeline from raw data to evaluated forecasts.

---

## Problem Statement

Delhi consistently ranks among the most polluted cities in the world. Accurate PM2.5 forecasting enables governments, healthcare systems, and citizens to take timely preventive action. This project addresses that need by developing and rigorously evaluating multiple time series forecasting approaches on ~2,000 daily observations spanning January 2015 to July 2020.

---

## Dataset

- **Source:** [Air Quality Data in India — Kaggle](https://www.kaggle.com/datasets/rohanrao/air-quality-data-in-india)
- **Origin:** India's Central Pollution Control Board (CPCB)
- **Scope:** Delhi daily PM2.5 readings, Jan 2015 – Jul 2020
- **Size:** ~2,009 observations after preprocessing

---

## Project Pipeline

**1. Data Preprocessing**
- Time-based linear interpolation for missing values
- Daily resampling to ensure temporal continuity
- Outlier clipping at 1st and 99th percentiles
- StandardScaler / MinMaxScaler normalization

**2. Feature Engineering**
- Cyclical encoding of month and day-of-week (sine/cosine transforms)
- Lag features at 1, 2, 3, 7, 14, and 30 days
- 7-day rolling mean and rolling standard deviation

**3. Exploratory Data Analysis**
- Trend and seasonal decomposition (annual period = 365 days)
- ACF / PACF analysis confirming strong lag-1 autocorrelation (~0.9)
- Pollutant correlation heatmap (PM2.5 vs PM10, NO2, AQI, CO, SO2, O3)

**4. Modeling**
- Chronological 70 / 15 / 15 train-validation-test split to prevent data leakage
- Five models implemented and evaluated: Naïve, Moving Average, ARIMA, SARIMA, LSTM

**5. Evaluation**
- Metrics: MAE, RMSE, MAPE, sMAPE
- Short-term vs long-term horizon analysis
- ARIMA rolling stability check over 200 observations

---

## Models & Results

| Model          | MAE    | RMSE      | sMAPE  |
|----------------|--------|-----------|--------|
| **LSTM**       | 13.74  | **18.31** | 77.36  |
| **Naïve**      | **11.84** | 29.67  | **29.74** |
| Moving Average | 16.32  | 36.37     | 44.99  |
| ARIMA(2,1,2)   | 65.35  | 95.98     | 52.93  |
| SARIMA         | 73.40  | 103.51    | 64.07  |

**Key findings:**
- **LSTM** achieved the best RMSE (18.31) and maintained consistent accuracy across both short-term and long-term forecast horizons (MAE: 15.4 vs 13.0)
- The **Naïve forecast** outperformed ARIMA and SARIMA on MAE, a direct result of the series' high day-to-day persistence
- **ARIMA and SARIMA** degraded significantly in multi-step forecasting, with SARIMA producing numerically unstable predictions exceeding 2,000 µg/m³ — roughly 5× actual observed values

---

## Tech Stack

| Area | Tools |
|---|---|
| Data Processing | Python, Pandas, NumPy |
| Visualization | Matplotlib, Seaborn |
| Statistical Models | Statsmodels (ARIMA, SARIMAX) |
| Deep Learning | TensorFlow / Keras (LSTM) |
| ML Utilities | Scikit-learn |

---

*IE7275 – Data Mining in Engineering*
