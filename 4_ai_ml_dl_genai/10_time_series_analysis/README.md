---
title: "Time Series Analysis"
category: ml
levels: ["mid", "senior", "principal"]
skills: [time-series, forecasting, arima, transformers]
questions:
  "mid": ["What is Stationarity?", "Explain ARIMA components.", "Difference between additive and multiplicative seasonality."]
  "senior": ["How does Prophet work?", "Explain LSTM for time series.", "What is 'Look-ahead Bias'?"]
  "principal": ["Discuss Temporal Fusion Transformers (TFT).", "How to handle multiple time series with hierarchy (e.g., store sales)?", "Explain Conformal Prediction for uncertainty quantification."]
---

# Time Series Analysis

Time Series forecasting is critical for business (demand planning, stock market, server load). It deals with data indexed by time.

## Core Concepts

### 1. Components of Time Series
*   **Trend**: Long-term increase or decrease.
*   **Seasonality**: Repeating patterns at fixed intervals (daily, weekly, yearly).
*   **Cyclical**: Fluctuations not of fixed period (business cycles).
*   **Noise (Residuals)**: Random variation.

### 2. Stationarity
*   A time series is stationary if its statistical properties (mean, variance, autocorrelation) do not change over time.
*   Most models (ARIMA) assume stationarity.
*   **Test**: Augmented Dickey-Fuller (ADF) test.
*   **Fix**: Differencing ($y_t - y_{t-1}$), Log transformation.

### 3. ARIMA (AutoRegressive Integrated Moving Average)
*   **AR(p)**: Uses past values to predict future. $y_t = \phi y_{t-1} + \dots$
*   **I(d)**: Differencing to make it stationary.
*   **MA(q)**: Uses past forecast errors. $y_t = \theta \epsilon_{t-1} + \dots$

## Advanced Topics

### 1. Prophet (Facebook)
*   Additive model: $y(t) = g(t) + s(t) + h(t) + \epsilon_t$.
*   Handles missing data, outliers, and holidays well.
*   Uses Fourier series for seasonality and piecewise linear growth for trend.

### 2. Deep Learning for Time Series
*   **RNN/LSTM**: Good for capturing sequential dependencies.
*   **TCN (Temporal Convolutional Networks)**: Dilated convolutions to capture long history. Parallelizable.
*   **Transformers**:
    *   **Temporal Fusion Transformer (TFT)**: State-of-the-art. Handles static covariates (store location), observed inputs (weather), and known future inputs (holidays). Interpretable attention.

### 3. Evaluation Metrics
*   **MAE**: Mean Absolute Error. Robust to outliers.
*   **RMSE**: Root Mean Squared Error. Penalizes large errors.
*   **MAPE**: Mean Absolute Percentage Error. Scale-independent. Fails if $y=0$.
*   **SMAPE**: Symmetric MAPE.

## Interview Questions

### Mid Level
1.  **What is Autocorrelation?**
    *   *Answer*: Correlation of a signal with a delayed copy of itself. Used to find repeating patterns (seasonality).
2.  **Why do we need to difference data?**
    *   *Answer*: To remove trend and seasonality, making the mean and variance constant (stationary), which is required for ARIMA models.
3.  **What is the difference between Univariate and Multivariate Time Series?**
    *   *Answer*: Univariate uses only the target variable's history. Multivariate uses other external variables (exogenous variables) to help prediction.

### Senior Level
1.  **Explain the concept of "Backtesting" in Time Series.**
    *   *Answer*: We cannot use random K-Fold CV because order matters. We use "Expanding Window" or "Rolling Window" validation. Train on past, test on immediate future, move window forward.
2.  **How does Prophet handle holidays?**
    *   *Answer*: It adds a dedicated component $h(t)$ which is a list of indicator functions (0 or 1) for specific dates. It learns a weight for each holiday.
3.  **What is Look-ahead Bias?**
    *   *Answer*: Using information from the future to predict the past. Common mistake: normalizing the entire dataset using global min/max before splitting into train/test.

### Principal Level
1.  **Design a demand forecasting system for a retail chain with 10,000 stores.**
    *   *Discussion*:
        *   **Hierarchy**: Global -> Region -> Store -> SKU.
        *   **Model**: Global model (TFT or DeepAR) trained on all series to learn shared patterns (cross-learning).
        *   **Features**: Static (Store ID, City), Past (Sales), Future (Promotions, Holidays).
        *   **Cold Start**: Use item metadata embeddings to predict for new items.
2.  **Explain Conformal Prediction.**
    *   *Discussion*: A framework to produce valid prediction intervals (e.g., "95% probability true value is in range") with statistical guarantees, without assuming a specific distribution.
3.  **Compare ARIMA vs LSTM.**
    *   *Discussion*: ARIMA: Linear, interpretable, good for single series, requires stationarity. LSTM: Non-linear, black-box, data-hungry, handles multiple series and complex interactions, no stationarity requirement.
