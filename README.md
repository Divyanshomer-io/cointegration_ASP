# Cointegration-Based Pairs Trading using Stochastic Process Models

## Assignment Overview

This repository demonstrates a quantitative pairs trading strategy grounded in modern stochastic process theory, using Indian pharmaceutical stocks as a real-world example. All statistical modeling, signals, and trading rules are explained with mathematical foundations and formulas.

## Stochastic Models and Logic

### 1. **Random Walks (Nonstationary Asset Prices)**
Financial asset prices \( P_t \) are assumed to follow a random walk:
\[
P_t = P_{t-1} + \varepsilon_t
\]
where \( \varepsilon_t \) is zero-mean noise. This property motivates the need for cointegration analysis.

### 2. **Engle-Granger Cointegration Model**
For each stock pair, we regress the log-price of stock \( A \) against stock \( B \):
\[
\log(P_A)_t = \alpha + \beta \log(P_B)_t + \varepsilon_t
\]
where \( \beta \) is the hedge ratio and \( \varepsilon_t \) is the spread.

### 3. **Spread Construction (Stationarity Testing)**
The spread for each pair is computed:
\[
S_t = \log(P_A)_t - \beta \log(P_B)_t
\]
Candidate pairs are selected by minimizing the average absolute spread.

### 4. **Augmented Dickey-Fuller (ADF) Test**
To confirm stationarity (mean-reverting dynamics), the spread \( S_t \) is tested:
\[
\Delta S_t = \alpha + \gamma S_{t-1} + \sum_{i=1}^p \delta_i \Delta S_{t-i} + \varepsilon_t
\]
A highly negative ADF statistic and \( p < 0.05 \) indicate a stationary spread, confirming cointegration.

### 5. **Ornstein-Uhlenbeck (OU) Mean-Reverting Process**
A stationary spread is modeled as an OU process:
\[
dS_t = \theta(\mu - S_t) dt + \sigma dW_t
\]
capturing mean reversion, with \( \mu \) as long-term mean and \( \theta \) as speed of reversion.

### 6. **Trading Signal Generation**
For the confirmed pair, we compute the rolling z-score of the spread:
\[
Z_t = \frac{S_t - \text{mean}_t}{\text{std}_t}
\]
Signals are based on empirical quantiles:
- **Long Entry:** \( Z_t < q_{0.05} \)
- **Short Entry:** \( Z_t > q_{0.95} \)
- **Exit:** When \( Z_t \) reverts toward the median.
Alternatively, thresholds can be optimized by maximizing the Sharpe ratio:
\[
\text{Sharpe} = \frac{\mathbb{E}[R]}{\text{Std}(R)}
\]

### 7. **Backtesting and Evaluation**
Portfolio performance is tracked with:
- **CAGR** (Compound Annual Growth Rate):  
  \(
  \text{CAGR} = \left( \frac{V_{\text{final}}}{V_{\text{start}}} \right)^{\frac{252}{\text{trading days}}} - 1
  \)
- **Max Drawdown**
- **Sharpe Ratio**

## How to Use

- Run `ASP_cointegration.ipynb` for stepwise pair selection, ADF stats, z-score logic, signal plots, and backtest results.
- All models and formulas are explained inline, for educational use and transparency.

## Project Files

- `ASP_cointegration.ipynb` — Main notebook
- `Technical-Report.docx` — Written explanation of models and results

## Academic Context

This project is part of coursework for Applied Stochastic Processes at BITS Pilani Goa, aimed at illustrating theoretical model application in financial engineering.

---

