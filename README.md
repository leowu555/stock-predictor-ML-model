# stock-predictor-ML-model

This project builds a machine learning pipeline to predict **next-day stock returns** using technical indicators derived from historical market data.  
The workflow includes feature engineering, regression and classification models, diagnostics, and a trading strategy backtest.

---

## Data Collection

Historical stock data is downloaded using the **yfinance API**.

Example ticker used in this project:

- NVDA (NVIDIA)

The dataset contains roughly **10 years of daily price data**, including:

- Adjusted Close
- Open / High / Low / Close
- Volume

---

## Feature Engineering

Technical indicators are created from price and volume data.

### Return Features
- Daily returns
- Lagged returns (1–10 days)
- Multi-day returns (5, 10, 20 days)

### Trend Indicators
- Moving averages (5, 10, 20, 50 days)
- Exponential moving averages

### Momentum Indicators
- Momentum relative to moving averages
- MACD (Moving Average Convergence Divergence)

### Volatility Indicators
- Rolling volatility
- Average True Range (ATR)
- Bollinger band width
- Z-score relative to moving average

### Oscillators
- RSI (Relative Strength Index)
- Stochastic oscillator

---

## Prediction Targets

Two prediction targets are used:

**Regression target**
- Predict next-day return

**Classification target**
- Predict whether the next-day return is positive or negative

---

## Train / Test Split

A chronological split is used to prevent look-ahead bias.

- 80% training data
- 20% testing data

---

## Models

### Ridge Regression

A regularized linear regression model is used as a baseline ML model.

Pipeline: StandardScaler → Ridge Regression


---

### Random Forest Regression

A non-linear ensemble model is trained to capture complex relationships between features and returns.

Hyperparameters are tuned using:

- RandomizedSearchCV
- TimeSeriesSplit cross-validation

---

### Random Forest Classification

A classification model predicts the **probability that the next-day return is positive**, which can be used to generate trading signals.

---

## Model Evaluation

Models are evaluated using:

- **MAE (Mean Absolute Error)** – prediction accuracy
- **R² score** – explained variance
- **Direction accuracy** – percentage of correct up/down predictions

---

## Model Diagnostics

Several visualizations are used to analyze model behavior:

- Price time series
- Return distribution
- Feature correlation matrix
- Actual vs predicted returns
- Residual plots

---

## Feature Importance

Random Forest feature importance is analyzed using:

- **Tree-based importance**
- **Permutation importance**

These help identify which indicators contribute most to predictive performance.

---

## Trading Strategy Backtest

Predictions are converted into trading signals.

**Regression signal**

- Long if predicted return > 0
- Short if predicted return < 0

**Probability strategy**

- Long if P(up) > 0.55
- Short if P(up) < 0.45
- Otherwise hold no position

---

## Transaction Costs

To make the simulation more realistic, transaction costs are included. cost_per_trade = 0.1%.


---

## Strategy Performance

Strategies compared:

- Buy and Hold
- Regression Signal Strategy
- Probability Threshold Strategy

Performance is evaluated using cumulative return curves.

---

## Risk Metrics

Strategies are evaluated using:

- **Sharpe Ratio**
- **Annualized Return**
- **Maximum Drawdown**

A **rolling Sharpe ratio** is also calculated to analyze stability over time.

---

## Technologies Used

- Python
- NumPy
- Pandas
- Matplotlib
- Scikit-learn
- yfinance

