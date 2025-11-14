# Cointegration-Based Pairs Trading Using Stochastic Process Models

This repository implements a complete **market-neutral pairs trading strategy** using:
- Engle–Granger cointegration,
- Augmented Dickey-Fuller (ADF) stationarity testing,
- Ornstein–Uhlenbeck (OU) mean-reversion modeling, and
- Data-driven quantile-based trading thresholds.

The project applies stochastic-process theory to **Indian pharmaceutical equities**, integrating rigorous mathematical modeling with practical trading signal engineering and backtesting.

---

## 1. Project Motivation

Financial asset prices typically follow **nonstationary random walks**, making direct forecasting difficult. However, certain asset pairs exhibit **long-run equilibrium relationships**, where a linear combination becomes stationary.

This project aims to:
- Identify such cointegrated pairs,
- Model the stationary spread using stochastic differential equations,
- Generate optimal trading signals, and
- Validate the strategy using historical price data.

---

## 2. Stochastic Models and Mathematical Framework

### 2.1 Random Walk Model for Individual Prices
Asset prices are assumed to follow:

$$
P_t = P_{t-1} + \varepsilon_t
$$

where \( \varepsilon_t \) is i.i.d. noise.
This motivates searching for **stationary combinations**, not stationary prices.

---

### 2.2 Engle–Granger Cointegration Model
To test whether two assets share a stable long-run relation, log-prices are regressed:
\[
\log P_{A,t} = \alpha + \beta \log P_{B,t} + \varepsilon_t
\]

Where:
- \( \beta \) = hedge ratio,
- \( \varepsilon_t \) = spread (candidate stationary process).

The spread is constructed as:
\[
S_t = \log P_A - \beta \log P_B
\]

---

### 2.3 ADF Test for Spread Stationarity
To confirm mean reversion, the ADF regression is applied:
\[
\Delta S_t = \alpha + \gamma S_{t-1} + \sum_{i=1}^p \delta_i \Delta S_{t-i} + u_t
\]

Decision rule:
- Reject \( H_0: \gamma = 0 \) (nonstationary)  
- Accept \( H_1: \gamma < 0 \) (stationary)

**Validation Example (CIPLA–LUPIN):**
- ADF statistic = **−3.45**
- p-value < **0.05**  
→ Spread is stationary and mean-reverting.

---

### 2.4 Ornstein–Uhlenbeck Model for Spread Dynamics
The stationary spread is modeled using an OU process:
\[
dS_t = \theta(\mu - S_t)\, dt + \sigma\, dW_t
\]

Where:
- \( \theta \) = mean-reversion speed  
- \( \mu \) = equilibrium mean  
- \( \sigma \) = volatility  
- \( W_t \) = Brownian motion  

This model supports timing of entries/exits for pairs trades.

---

## 3. Signal Engineering & Threshold Optimization

### 3.1 Z-Score Normalization
To standardize spread deviations:
\[
Z_t = \frac{S_t - \overline{S}}{\sigma_S}
\]

---

### 3.2 Empirical Quantile-Based Thresholds
Instead of fixed ±1.5 std thresholds, this project derives thresholds from in-sample spread distribution:

- **Long Entry:**  
  \[
  Z_t < q_{0.05}
  \]

- **Short Entry:**  
  \[
  Z_t > q_{0.95}
  \]

- **Exit:**  
  Reversion toward median  
  \[
  Z_t \approx q_{0.50}
  \]

**Derived Thresholds (2017–2020):**

| Threshold Type | Value |
|----------------|--------|
| Lower (5%) | **−1.86** |
| Upper (95%) | **2.64** |
| Exit (median) | **0.83** |

These values optimize signal sensitivity and reduce noise.

---

## 4. Implementation Pipeline

### 4.1 Data
- Universe: **10 Indian pharma stocks**
- Training: **2017–2020**
- Testing/Backtest: **2020–2023**

### 4.2 Pair Selection
Steps:
1. OLS regression to estimate hedge ratios  
2. Generate residual spread  
3. Apply ADF test  
4. Rank pairs by stationarity strength  

**Best Pair Identified:**  
**CIPLA–LUPIN**

---

## 5. Backtesting Framework

- Rolling z-score computation  
- Long/short entries based on empirical thresholds  
- Exit upon convergence  
- Daily PnL updates using hedge ratio  
- Performance measured using cumulative returns and Sharpe ratio  

---

## 6. Results

### 6.1 Statistical Validation
- CIPLA–LUPIN spread confirmed stationary  
- Strong OU-like mean reversion  
- ADF Test:  
  - Statistic = **−3.45**  
  - p-value < **0.05**

### 6.2 Backtest Performance (2021–2023)
- Market-neutral strategy  
- Approx **2.5% cumulative return** over 3 years  
- Low volatility due to hedged structure  

---
## 7. Repository Structure

.
├── ASP_cointegration.ipynb # Complete analysis, modeling, and backtests
├── Technical-Report.docx # Formal academic writeup
├── ASP-assignment_Cointegration.pdf # Reference PDF submitted for coursework
└── README.md # Project documentation

---

## 8. Academic Context

This project was developed as part of the  
**Applied Stochastic Processes (ASP)** course at  
**BITS Pilani, K.K. Birla Goa Campus**.

It demonstrates:
- Integration of stochastic calculus principles  
- Econometric modeling (ADF, cointegration)  
- Mean-reversion dynamics via OU process  
- Algorithmic trading signal generation  
- Practical backtesting and validation  

---

## 9. How to Run

1. Clone the repository  
2. Open and run `ASP_cointegration.ipynb` sequentially  
3. Review:  
   - Hedge ratio estimation  
   - ADF stationarity tests  
   - OU diagnostics  
   - Z-score threshold generation  
   - Strategy backtest  
4. Inspect generated plots and performance tables  

---

## 10. Summary

This repository provides a fully reproducible and mathematically rigorous demonstration of **stochastic-process-based pairs trading**, unifying theory and practice through a structured pipeline:
1. Random walks → 2. Cointegration → 3. Stationarity tests →  
4. OU modeling → 5. Z-score normalization → 6. Quantile thresholds →  
7. Backtesting.

It serves as an educational as well as practical reference for financial engineering, quantitative trading, and applied stochastic modeling.

---


