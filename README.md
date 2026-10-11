# Multi-Asset Geometric Brownian Motion (GBM) Portfolio Simulator

A advanced financial engineering script that uses a **Multivariate Geometric Brownian Motion (GBM)** model to execute Monte Carlo simulations for multi-asset stock portfolios. By calculating a covariance matrix and using a Cholesky decomposition, the script captures historical assets correlation to forecast future portfolio behavior.

## Core Features

*   **Correlated Random Walks:** Utilizes Cholesky decomposition (L) on the historical covariance matrix to generate correlated random shocks (Z) across multiple assets simultaneously.
*   **Geometric Brownian Motion:** Models asset price trajectories using continuous stochastic processes based on asset drifts (historical log return means) and historical volatility.
*   **Advanced Risk Metrics:** Automatically quantifies downside tail risk using **Value at Risk (VaR)** and **Expected Shortfall / Conditional VaR (CVaR)** at customizable confidence parameters.
*   **Real Return Tracking:** Calculates both nominal expected portfolio returns and inflation-adjusted real returns based on a designated simulation window.
*   **Granular Visual Projections:** Automatically renders independent trend charts for the combined portfolio values as well as standalone performance plots for every individual ticker tracking against its median outcome.

## Getting Started

### Prerequisites

Ensure you have the required analytical and charting libraries installed:

```bash
pip install numpy pandas yfinance matplotlib
```

### Usage Example

```python
from datetime import datetime
# Import or paste the function here

# Run a 50/50 simulation for Apple and Nvidia across 1,000 randomized paths
geometric_brownian_motion_monte_carlo_simulation(
    portfolio_worth=10000,
    stocks=["AAPL", "NVDA"],
    weights=[0.5, 0.5],
    sim=1000
)
```

## Function Arguments & Parameters

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `portfolio_worth` | `float` | `10000` | Initial dollar capital allocation value used to distribute weights. |
| `sim` | `int` | `1000` | The total number of separate randomized simulation paths to evaluate. |
| `start` | `str` | `'2020-01-01'` | Start date for pulling historical pricing benchmarks (`YYYY-MM-DD`). |
| `end` | `str` | *Dynamic (Today)* | End date for historical data pull. Defaults to the current date. |
| `stocks` | `list` | `["AAPL", "NVDA"]` | Array of yFinance ticker symbols composing the active portfolio. |
| `weights` | `list` | `[0.5, 0.5]` | Capital weighting breakdown per ticker. Must sum up to `1.0`. |
| `days_simulated` | `int` | `252` | Trading days into the future to forecast (252 days ≈ 1 year). |
| `value_for_probability_calculation` | `float` | `50` | Targeted portfolio-level percentage growth hurdle rate. |
| `inflation_rate` | `float` | `2` | Annualized inflation penalty percentage applied to future expected values. |
| `VaR` | `float` | `95` | Statistical confidence level percentile threshold for VaR and CVaR calculations. |
| `best_outcome_percent` | `float` | `5` | The top percentile parameter tracking upside outliers. |

## Explaining the Outputs & Risk Metrics

The script outputs terminal report metrics grouped under `Portfolio` and individual stock categories:

*   **Median Return:** The 50th percentile mark showing the mid-point return of the portfolio paths.
*   **Expected Return:** The mathematical average return across all generated trajectories.
*   **Real Expected Return:** Expected percentage return factoring in the purchasing power erosion of your `inflation_rate`.
*   **Value at Risk (VaR):** The threshold loss percentage that will not be exceeded with the chosen confidence level (e.g., *“There is a 95% chance our losses will not exceed X%”*).
*   **Expected Shortfall (CVaR):** The average percentage loss *if* the portfolio falls into the worst-case scenario tail beyond the VaR threshold. 
*   **Probability of Profit:** The likelihood of your total ending asset value scaling past the initial baseline net of transaction fee friction thresholds (`* 0.9899`).
