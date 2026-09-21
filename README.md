# Portfolio-Risk-Forecasting

This repository contains the implementation accompanying the study **“A Distributional Similarity Framework for Portfolio Risk Forecasting in Equity Markets.”**

The study develops and empirically evaluates a **distributional similarity framework** for equity benchmark comparison, distributional risk profiling, and out-of-sample portfolio forecasting across static and dynamic horizons. The framework combines conventional dependence and volatility measures with non-parametric distributional tests, distributional distance measures, distributional shape characteristics, and tail-risk measures.

## Overview

The framework examines whether an individual equity exhibits distributional similarity to a relevant **sector or market benchmark**, going beyond conventional correlation and volatility measures.

The empirical pipeline integrates:

* **Dependence and volatility measures:** Pearson correlation and annualized volatility
* **Distributional tests:** Kolmogorov--Smirnov (KS), Anderson--Darling (AD), Cramér--von Mises (CvM), and Epps--Singleton
* **Distributional distance:** Energy Distance (ED)
* **Distributional shape:** conventional moments and L-moments
* **Tail characteristics:** Hill-type tail indices and left--right tail asymmetry
* **Risk measures:** Value-at-Risk (VaR) and Expected Shortfall (ES)
* **Risk profiling:** hierarchical clustering and silhouette validation
* **Portfolio evaluation:** in-sample, static out-of-sample, and rolling-origin forecasting
* **Implementation robustness:** annual rebalancing, turnover, and transaction-cost sensitivity

## Return Representations

The distributional comparisons are evaluated under three alternative representations of recent returns:

1. **Raw returns**
2. **Empirical Z-score standardized returns**
3. **AR(1)-GARCH(1,1) standardized residuals with Student-\(t\) innovations**

The rolling-origin forecasting experiment uses a 126-trading-day training window and a 21-trading-day forecasting horizon. Portfolio weights are re-estimated at each forecasting origin using only information available up to that point.

## Portfolio Construction

The portfolio construction procedures use combinations of Energy Distance, distributional-test statistics, tail characteristics, correlation, and volatility.

For example, the baseline Energy Distance specification assigns weights inversely proportional to distributional distance:

$$
w_i^{(ED)}
=\frac{D_i^{-1}}
{\sum_{j=1}^{N}D_j^{-1}},
$$

where $$D_i$$ denotes the Energy Distance between stock \(i\) and its corresponding benchmark.

For distributional-test-based specifications, test \(p\)-values are incorporated as **monotonic similarity scores** within the weighting rule. Higher \(p\)-values correspond to weaker evidence against distributional equality and therefore receive larger allocation scores. This use of \(p\)-values is intended as a transparent heuristic weighting mechanism rather than as an interpretation of \(p\)-values as effect sizes or probabilities of distributional equality.

## Data

The empirical analysis uses daily financial data for **18 large-cap U.S. equities across six sectors**:

| Sector                 | Equities         | Sector ETF |
| ---------------------- | ---------------- | ---------- |
| Technology             | AAPL, MSFT, NVDA | XLK        |
| Consumer Discretionary | TSLA, AMZN, HD   | XLY        |
| Financials             | JPM, BAC, GS     | XLF        |
| Energy                 | XOM, CVX, COP    | XLE        |
| Health Care            | JNJ, UNH, PFE    | XLV        |
| Consumer Staples       | WMT, PG, KO      | XLP        |

Three broad market ETFs are also used as market-level benchmarks:

* **SPY** — S&P 500
* **QQQ** — NASDAQ-100
* **DIA** — Dow Jones Industrial Average

Daily closing prices are obtained from **Yahoo Finance** using the `yfinance` Python library, and continuously compounded returns are calculated as

$$
r_{i,t}=\ln\left(\frac{P_{i,t}}{P_{i,t-1}}\right).
$$

## Empirical Evaluation

The repository implements a sequential empirical design consisting of:

### 1. Static Risk and Tail Characterization

Unconditional portfolio and distributional characteristics are examined using VaR, Expected Shortfall, conventional moments, L-moments, Hill-type tail indices, and tail asymmetry.

### 2. Distributional Similarity Analysis

Individual equities are compared with their sector and market benchmarks using correlation, distributional tests, and Energy Distance.

### 3. Rolling Distributional Similarity

A 252-trading-day rolling analysis evaluates the temporal evolution of lower-tail similarity and overall filtered distributional separation.

### 4. Distributional Risk Profiling

Hierarchical clustering based on Energy Distance and L-kurtosis is used to identify low-, intermediate-, and high-distributional-risk profiles. Silhouette analysis is used to assess cluster separation.

### 5. Portfolio Forecasting

Alternative portfolio specifications are evaluated using:

* Full-sample in-sample characterization
* Three-year training / approximately two-year static out-of-sample evaluation
* Rolling-origin forecasting with six months of training and one month of forecasting

Random long-only portfolios generated from a Dirichlet distribution are used as performance benchmarks.

### 6. Portfolio Implementation Robustness

The static out-of-sample portfolios are additionally evaluated under annual rebalancing with natural portfolio-weight drift. Turnover and transaction-cost sensitivity are examined using alternative proportional transaction-cost assumptions.

### 7. Distributional Risk Profile and OOS Allocation Cross-Check

The distributional risk profiles obtained from hierarchical clustering are compared with subsequent out-of-sample portfolio allocation patterns to assess the internal consistency between cross-sectional distributional profiling and portfolio weighting behavior.

## Main Empirical Finding

Across the examined rolling-origin specifications, empirical Z-score normalization generally provides stronger risk-adjusted performance for several distributional-similarity-based strategies than raw returns and AR(1)-GARCH(1,1) standardized residuals.

The strongest reported specification combines:

**Energy Distance + Epps--Singleton + Tail Asymmetry**

under empirical Z-score normalization, achieving an out-of-sample annualized Sharpe ratio of **1.3233** and outperforming **92.6% of dynamically regenerated random portfolios** in the examined sample.

The results also show that distributional similarity does not uniformly dominate conventional correlation- and volatility-based approaches, and that the forecasting contribution of individual distributional measures depends on the statistical representation and forecasting horizon.

## Reproducibility

The repository is intended to support reproducibility of the empirical analysis. The experiments use deterministic/random seed settings where stochastic procedures are involved, including origin-specific seeds for dynamically generated rolling benchmark portfolios.

The implementation also includes numerical checks for non-finite observations, invalid parameter estimates, insufficient observations, and failed AR(1)-GARCH(1,1) estimation.

## Software Environment

The analysis is implemented in Python using commonly used scientific and statistical libraries, including:

* NumPy
* Pandas
* SciPy
* Statsmodels
* ARCH
* Scikit-learn
* Matplotlib
* Seaborn
* yfinance

## Citation

If you use this repository or the associated methodology in academic work, please cite the corresponding paper:

> **A Distributional Similarity Framework for Portfolio Risk Forecasting in Equity Markets**

A formal citation entry will be provided once the paper is published.

