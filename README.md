# Workforce Demand Forecasting

Time series forecasting project for workforce planning using SARIMA and LightGBM, with walk-forward backtesting, model evaluation, and prediction intervals.

## Project Overview

This project analyzes daily workforce demand and develops forecasting models to estimate future staffing requirements.

The workflow includes time series diagnostics, classical forecasting, machine learning, walk-forward backtesting, uncertainty estimation, and final model selection.

## Dataset

The project uses the `workforce_demand.csv` dataset provided as part of the course.

- **Frequency:** Daily
- **Period:** January 2024 – December 2025
- **Observations:** 731
- **Target:** `required_headcount`
- **Missing values:** None
- **Duplicate rows:** None

A structural break occurs around **April 1, 2025**, where the workforce demand level changes noticeably.

## Workflow

```text
Historical Workforce Data
        ↓
Time Series Diagnostics
        ↓
STL Decomposition
        ↓
ACF / PACF + ADF Test
        ↓
SARIMA & LightGBM
        ↓
Walk-Forward Backtesting
        ↓
MAE / RMSE / WAPE
        ↓
Prediction Intervals
        ↓
Model Comparison
        ↓
30-Day Forecast
```

## Time Series Analysis

The series was analyzed using:

- STL decomposition
- Trend, seasonality, and residual analysis
- ACF and PACF
- Augmented Dickey-Fuller test
- First-order differencing

The original series was non-stationary. First-order differencing produced a stationary series and supported the use of `d = 1` in the SARIMA model.

## Forecasting Models

### SARIMA

Candidate SARIMA models were compared using AIC.

The selected specification was:

`SARIMA(1,1,1)(1,1,1,7)`

Residual diagnostics were evaluated using the Ljung-Box test.

### LightGBM

The LightGBM model used:

- Calendar features
- Lag features
- Rolling features

All lag and rolling features were constructed using historical values only to prevent data leakage.

## Backtesting

An expanding-window walk-forward validation strategy was used with three 30-day validation periods:

- Before the structural break
- During the structural break
- After the structural break

The same validation periods were used for both SARIMA and LightGBM.

## Model Performance

| Model | MAE | RMSE | WAPE |
|---|---:|---:|---:|
| SARIMA | 10.95 | 12.43 | 13.05% |
| LightGBM | 12.29 | 13.85 | 14.58% |

SARIMA achieved lower average forecasting errors across the three walk-forward folds.

## Prediction Intervals

SARIMA 95% prediction intervals were evaluated using:

- Empirical coverage
- Average interval width

The average empirical coverage was approximately **65.56%**, compared with the nominal **95%** level.

Coverage decreased substantially during the structural break, indicating that abrupt regime changes remain difficult to capture using historical patterns alone.

## Final Model

SARIMA was selected as the final model based on:

- Lower MAE, RMSE, and WAPE
- Strong weekly seasonal structure
- Interpretability
- Native prediction interval support
- Lower feature engineering requirements
- Relatively low computational complexity

## Final Forecast

The selected SARIMA model was retrained on the complete historical dataset and used to generate a **30-day workforce demand forecast** with **95% prediction intervals**.

## Technologies

`Python` · `Pandas` · `NumPy` · `Matplotlib` · `Statsmodels` · `LightGBM` · `Scikit-learn` · `Google Colab`

## Repository Structure

```text
workforce-demand-forecasting/
│
├── workforce_demand_forecasting.ipynb
├── README.md
└── .gitignore
```

## How to Run

1. Open `workforce_demand_forecasting.ipynb`.
2. Run the notebook in Google Colab.
3. Execute all cells from top to bottom.

The dataset is loaded directly from the course repository.

## Program

Developed as part of the **Time Series Analysis & Forecasting** program at [SDAIA Academy](https://github.com/SDAIAAcademy).

## Author

**Lujain Alrimali**
