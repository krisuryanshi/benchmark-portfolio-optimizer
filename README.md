# Benchmark Portfolio Optimizer 📊  
**CFM 101 – Robo-Advising Challenge (Market Meet Strategy)**

> For the complete implementation and outputs, refer to `main.ipynb`.

## Overview
This project builds a rule-based portfolio optimizer that constructs a diversified equity portfolio designed to track a benchmark defined as the average of the S&P 500 and TSX Composite total returns.

Everything is implemented programmatically and follows the assignment constraints end to end. The final result is a fee-adjusted portfolio that can be executed using fractional shares.

**Competition Goal:** Market Meet

---

## Repository Structure
```
benchmark-portfolio-optimizer/
│
├── main.ipynb          # Main analysis and portfolio construction notebook
├── tickers.csv         # Input ticker universe
├── portfolio.csv       # Final output (Ticker, Shares)
├── requirements.txt    # Required Python libraries
└── README.md           # Project documentation
```

---

## Requirements
The project uses the following Python libraries:
```
pandas
numpy
yfinance
matplotlib
scipy
```

---

## Strategy Summary
The portfolio construction process follows five main steps:

1. Ticker filtering  
2. Benchmark construction  
3. Portfolio selection  
4. Tracking error optimization  
5. Fee-adjusted execution  

Each step is designed to satisfy the assignment rules while keeping the portfolio closely aligned with the benchmark.

---

## Ticker Filtering
Tickers are read from `tickers.csv` and filtered using the following criteria:

- Valid historical price data between October 1, 2024 and September 30, 2025  
- Average daily trading volume of at least 5,000 shares  
- Months with fewer than 18 trading days are excluded from volume calculations  
- Duplicate and invalid tickers are removed  

Only tickers that pass all checks are kept for further analysis.

---

## Date Range Selection
A three-year analysis window was used:

- Start: November 21, 2022  
- End: November 21, 2025  

This range captures recent market behavior while avoiding early COVID-era distortions and includes higher-volatility periods in 2024–2025.

---

## Benchmark Construction 📈
The benchmark is defined as the average of cumulative total returns from:

- S&P 500  
- TSX Composite  

Daily closing prices are converted to daily returns, accumulated into cumulative return series, and averaged to form the benchmark.

---

## Stock Ranking via Distance to Benchmark
For each filtered stock:

- Cumulative total returns are calculated  
- The average absolute distance from the benchmark’s cumulative return is computed  

Stocks are ranked by this distance, where lower values indicate closer tracking. Cross-listed duplicates are removed, keeping the version that tracks the benchmark more closely.

---

## Portfolio Construction Rules
A portfolio of 20 stocks is selected under the following constraints:

- At least one large-cap stock (market cap > $10B CAD)  
- At least one small-cap stock (market cap < $2B CAD)  
- No sector exceeding 40 percent of the portfolio  
- Preference given to stocks with smaller benchmark distance  

Market capitalizations are converted to CAD using the live USD/CAD exchange rate.

---

## Covariance and Benchmark Relationships
Using aligned daily returns:

- A covariance matrix for the selected stocks is computed  
- Covariances between each stock and the benchmark are calculated  
- Benchmark variance is computed  

These values are used as inputs for the optimization step.

---

## Portfolio Optimization 🧮
The portfolio is optimized by minimizing tracking error variance using standard portfolio theory.

A beta penalty term is included to keep the portfolio’s beta close to 1 relative to the benchmark.

Constraints enforced during optimization:

- Weights sum to 100 percent  
- Minimum weight per stock equal to 100 divided by twice the number of stocks  
- Maximum weight per stock of 15 percent  
- Sector exposure capped at 40 percent  

Initial weights are distance-based, giving higher starting weights to stocks that track the benchmark more closely.

---

## Post-Optimization Checks
After optimization, the portfolio is checked to confirm:

- Sector exposure remains within limits  
- Both large-cap and small-cap exposure are present  
- Final weights remain valid after numerical optimization  

Sector allocations and market-cap exposure are reported and visualized.

---

## Fee-Adjusted Portfolio Execution 💸
The optimized weights are converted into trades using a $1,000,000 CAD budget.

- USD-denominated stocks are converted using the live exchange rate  
- Transaction fees are applied as either $2.15 USD flat or $0.001 USD per share, whichever is smaller  
- Fees are deducted before final allocation  
- Portfolio values are rescaled proportionally after fees  

Fractional shares are used to deploy capital efficiently.

---

## Final Output
The final portfolio is exported as `portfolio.csv`, containing:

- Ticker  
- Shares purchased  

The final portfolio value is as close as possible to $1,000,000 CAD after fees, with weights summing to 100 percent.

---

## My Contributions
*This project was completed as part of a group assignment. The section below describes my individual contributions.*

**Krish Suryavanshi**

- Designed and implemented the ticker filtering and liquidity screening pipeline  
- Selected the historical date range for analysis  
- Designed the benchmark distance ranking method  
- Built the portfolio selection logic under sector and market-cap constraints  
- Implemented post-optimization validation checks  

---

## Notes
- The code avoids hard-coding and is designed to run on any valid ticker set  
- All assignment constraints are enforced programmatically  
- Contest-period performance does not affect grading  
