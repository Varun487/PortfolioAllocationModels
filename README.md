# Portfolio Allocation Models Comparision

## Aim

The aim of this study is to build and compare 2 variations of factor-based long/short allocation models with constraints on their betas.

## Project description

### Data

For this study we have used the following ETFs, downloaded from Yahoo finance, dating from March $1^{st}, 2007$ to October $31^{st}, 2022$.

- Currency Euro Shares (FXE)
- iShares MSCI Japan Index (EWJ)
- SPDR GOLD Trust (GLD)
- Powershares NASDAQ-100 Trust (QQQ)
- SPDR SP 500 (SPY)
- iShares Lehman Short Treasury Bond (SHV)
- PowerShares DB Agriculture Fund (DBA)
- United States Oil Fund LP (USO)
- SPDR SP Biotech (XBI)
- iShares SP Latin America 40 Index (ILF)
- SPDR SP Emerging Middle Est Africa (GAF)
- iShares MSCI Pacific ex-Japan Index Fund (EPP)
- SPDR DJ Euro Stoxx 50 (FEZ)

Our benchmark is the SPY (from the same time periods).

We have also downloaded the French-Fama factors for the aboove time periods in building our model.

### Time periods in consideration

This study also seeks to analyze the returns of the optimization strategies under various real
world market conditions, specifically, the following time periods:
1. Before Subprime crisis (1st March, 2007 to 31st December, 2008)
2. During Subprime crisis (1st January, 2009 to 31st December, 2009)
3. After Subprime crisis and recover(1st January, 2010 to 31st December, 2019)
4. During Covid Virus pandemic (1st January, 2020 to 31st December, 2020)
5. After Covid Virus Pandemic (1st January, 2021 to 31st January, 2022)
6. War, recession and recent economic climate(1st February, 2022 to 31st October, 2022)

### The first model

The first strategy considers a target Beta in the interval [−0.5, 0.5], with a Value-at-Risk type of utlitity corresponding to Robust Optimization and is defined as

$$
\begin{cases}
\max\limits_{{\omega ∈ ℝ^{n}}}\rho^{T}\omega-\lambda\sqrt{\omega^{T}\Sigma\omega}\\
-0.5\le\sum_{i=1}^{n} \beta_{i}^{m}\omega_{i}\le0.5\\
\sum_{i=1}^{n} \omega_{i}=1, -2\leq\omega_{i}\leq2
\end{cases}
$$

### The second strategy

The second strategy considers a target Beta in the interval [−1, +2] and incorporates an Information Ratio term to limit the deviations from a benchmark (SPY ETF) unless those deviations yield a high return.

$$
\begin{cases}
\max\limits_{{\omega ∈ ℝ^{n}}}\frac{\rho^{T}\omega}{TEV(\omega)} -\lambda\sqrt{\omega^{T}\Sigma\omega}\\
-1\le\sum_{i=1}^{n} \beta_{i}^{m}\omega_{i}\le2\\
\sum_{i=1}^{n} \omega_{i}=1, -2\leq\omega_{i}\leq2
\end{cases}
$$

- $\omega$ is the portfolio weights to be maximized and recomputed during rebalancing the portfolio (weekly)
- $\Sigma$ is the the covariance matrix between the securities returns (computed from French Fama 3-factor Factor Model)
- $\lambda$ is a small regularization parameter to limit the turnover
- $\rho$ is the expected returns vector computed according to the period of the estimator used for this model
- $\beta_{i}^{m}=\frac{cov(r_{i},r_{M})}{\sigma^{2}(r_{M})}$ is the Beta of security $S_{i}$ as defined in the CAPM Model so that $\beta_{P}^{m}=\sum_{i=1}^{n}\beta_{i}^{m}\omega_{i}$ is the Beta of the Portfolio
- TEV($\omega$) is the Tracking Error Volatility = $\sigma(r_{P}(\omega)-r_{SPY})$

We compare the outcomes of the two models while evaluating their sensitivity to the the length of the estimators for covariance matrix and the expected returns under different market scenarios - namely before during and after the subprime mortgage crisis and the covid-19 crisis.

The portfolios will be reallocated (re-balanced) every week for period of analysis from March 2007 to end of October 2022.

### Estimators

We use trend following estimators for computing the Expected Returns vector ($\rho$) and the Covariance Matrix ($\sigma$). 

Furthermore, we consider 3 lookback periods for our estimators:
- Long-Term estimator (LT, 120 days)
- Mid-Term estimator (MT, 90 days)
- Short-Term estimator ( ST, 60 days)

The behavior of the optimal portfolio built from a specific combination of estimators for Covariance and Expected Return may change with the Market environment, a particular strategy being defined by a specific combination,
for example the notation $S_{90}^{60}$ - can be used to say that you are using 60 days estimation of covariance, 90 days for estimation of Expected Returns.

In this project, we consider the following estimators
- $S_{60}^{60}$
- $S_{60}^{90}$
- $S_{60}^{120}$
- $S_{90}^{60}$
- $S_{90}^{90}$
- $S_{90}^{120}$
- $S_{120}^{60}$
- $S_{120}^{90}$
- $S_{120}^{120}$

### Backtest

We backtest all above model combinations on the data.

### Computing and reporting metrics

We compute the following metrics post running backtests:
- Annual returns
- Volatility
- Sharpe Ratio
- Kurtosis
- Skewness
- Max drawdown
- C VaR
- VaR
- Mean daily return
- Geometric daily return

## Build the project

1. Initialize the project

```bash
git clone git@github.com:Varun487/PortfolioAllocationModels.git
cd PortfolioAllocationModels
pip3 install -r requirements.txt
```

2. Run the jupyter notebook `Portfolio Allocation Models Comparision.ipynb`
