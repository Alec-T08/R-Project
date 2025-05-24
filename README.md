
# 📊 R Project 2025 – Backend Documentation

Hi [Name],

Here’s a breakdown of the R backend for our portfolio analysis and optimization tool. This is meant to be connected to a Shiny app or dashboard that allows users to explore stock data and get personalized portfolio suggestions based on their risk preferences.

---

## 🔧 What the Script Does

This backend performs the following:

1. **Downloads stock price data** (Yahoo Finance) for a user-selected list of tickers.
2. **Calculates monthly returns** and basic statistics.
3. **Builds company-level objects** for querying info like volatility, returns, etc.
4. **Computes optimized portfolios** using a simplified mean-variance model.
5. **Supports user inputs** like risk aversion, investment budget, and time window.

---

## 🧩 User Inputs (Configurable)

These variables are defined at the top of the script:

```r
tickers         # 20 stocks, mix of risky and stable
from            # Start date (e.g. "2015-01")
to              # End date (e.g. "2025-04")
budget          # Total investment budget (e.g. 100000)
Rf              # Risk-free rate (e.g. 0.023 or 2.3%)
risk_aversion   # Between 0 (aggressive) and 1 (conservative)
```

---

## 📉 Data Flow Overview

### 1. **Price Data Retrieval**
Function `get_prices_multiple()` downloads **monthly adjusted close prices** for all tickers and merges them into one dataset, handling missing tickers gracefully.

```r
all_prices <- get_prices_multiple(tickers, from, to)
```

### 2. **Return Calculation**
We compute **monthly returns** using:
```r
monthly_returns <- na.omit(Return.calculate(all_prices))
```

---

## 📊 Company-Level Statistics

The `stats` data frame includes:
- Mean monthly return
- Standard deviation (volatility)
- Best and worst monthly return
- Most inversely correlated stock

Each company is wrapped as a `Company_Info` object and stored in `company_objects`. You can access data like this:

```r
company_objects$AAPL
print(company_objects$NVDA)
```

---

## 🔍 Helper Functions for UI Use

These functions return single company objects for:
- Highest mean return: `get_highest_mean_return()`
- Most volatile asset: `get_most_volatile()`
- Biggest monthly loss: `get_biggest_loss()`
... and similar.

You can display this info dynamically in the UI.

---

## 💼 Portfolio Optimization

Main function:
```r
optimize_portfolio(returns, cov_matrix, risk_aversion, Rf, budget)
```

This blends:
- **Minimum variance portfolio** (low risk)
- **Maximum return portfolio** (greedy)
Based on the `risk_aversion` value (slider in the UI).

It returns:
- Weight for each asset
- Allocation of budget
- Expected return and volatility
- Sharpe ratio

Example output:
```r
$result$allocation      # dollar values
$result$weights         # portfolio proportions
$result$expected_return # e.g. 0.0154
```

---

## ⚠️ Known Limitations

- Some newer stocks (e.g., PLTR, RIVN) have **incomplete history**. These NAs can distort the optimization.
- To-do: Add logic to **filter out tickers** with too many missing values before optimization.

---

## ✅ Next Steps (Frontend Ideas)

You can build these in the UI:
- Dropdown: Show info for a selected company
- Slider: Risk preference ➜ passed as `risk_aversion`
- Portfolio chart: Pie chart or bar of allocations
- Highlight: Best/worst performer, most volatile
- Option to re-optimize based on user filters

---

Let me know if you need sample JSON structures, endpoint suggestions, or hooks for Shiny events. Happy to tweak the backend further based on your needs.

Cheers,  
[Your Name]
