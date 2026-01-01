# Benchmark Portfolio Optimizer 📊  
**CFM 101 – Robo-Advising Challenge (Market Meet Strategy)**

> Full implementation and outputs are in `main.ipynb`.

## Overview
This project builds a rule-based portfolio optimizer that constructs a diversified equity portfolio designed to track a benchmark defined as the average of S&P 500 and TSX Composite total returns.

All logic is implemented in code and follows the assignment constraints throughout. The final result is a fee-adjusted portfolio that can be executed using fractional shares.

**Competition goal:** Market Meet

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
The optimizer reads an input universe from `tickers.csv` and follows this process:

- Filter eligible stocks based on data quality and liquidity  
- Construct a benchmark from S&P 500 and TSX Composite total returns  
- Rank stocks by average distance from the benchmark  
- Select and optimize a 20-stock portfolio under sector and market-cap constraints  
- Execute the portfolio with transaction fees and fractional shares  

---

## Ticker Filtering
Stocks are filtered using these rules:

- Valid historical data from October 1, 2024 to September 30, 2025  
- Average daily volume of at least 5,000 shares  
- Months with fewer than 18 trading days excluded  
- Duplicate and invalid tickers removed  

---

## Analysis Window
A three-year window is used:

- November 21, 2022 to November 21, 2025  

This captures recent market behavior while avoiding early COVID-period distortions and includes higher volatility in 2024–2025.

---

## Benchmark and Stock Ranking
The benchmark is defined as the average of cumulative total returns from the S&P 500 and TSX Composite.

Each stock is ranked by its average absolute distance from the benchmark’s cumulative return. Cross-listed duplicates are removed, keeping the version that tracks the benchmark more closely.

---

## Portfolio Construction and Optimization
A portfolio of 20 stocks is selected with the following constraints:

- At least one large-cap stock (market cap > $10B CAD)  
- At least one small-cap stock (market cap < $2B CAD)  
- No sector above 40 percent exposure  
- Maximum weight of 15 percent per stock  

Weights are optimized by minimizing tracking error variance, with a beta penalty to keep portfolio beta close to 1. Initial weights favor stocks that track the benchmark more closely.

---

## Fee-Adjusted Execution
The optimized portfolio is executed using a $1,000,000 CAD budget.

- USD stocks use the live USD/CAD exchange rate  
- Fees applied as $2.15 USD flat or $0.001 USD per share, whichever is smaller  
- Portfolio weights are rescaled after fees  
- Fractional shares are used to deploy capital efficiently  

---

## Final Output
The final portfolio is exported to `portfolio.csv`, containing each ticker and the number of shares purchased. Final weights sum to 100 percent, with total value as close as possible to $1,000,000 CAD after fees.

---

## My Contributions
*Group assignment. Contributions listed below are my own.*

**Krish Suryavanshi**
- Built the ticker filtering and liquidity screening logic  
- Designed the benchmark distance ranking method  
- Implemented portfolio selection under sector and market-cap constraints  
- Added optimization validation checks  

---

## Notes
- All constraints are enforced in code  
- No values are hard-coded to a fixed ticker set  
- Contest-period performance does not affect grading
