# Opening Range Breakout (ORB) Backtesting

## Overview

This project implements and backtests two Opening Range Breakout (ORB) trading strategies using Python and `backtesting.py`:

* **ORB_15:** Uses the first 15 minutes of the regular trading session.
* **ORB_30:** Uses the first 30 minutes of the regular trading session.

The backtest uses 1-minute OHLCV data and a volume delta filter to evaluate breakout signals.

## Strategy Rules

### Opening Range

* Session starts at **09:30 AM New York time**.
* Calculate the highest high and lowest low during the opening range.
* Accumulate volume delta throughout the opening range.

### Volume Delta

Volume delta estimates buying and selling pressure using each candle's closing price relative to its high and low.

```text
Buy Volume  = Volume × (Close − Low) / (High − Low)
Sell Volume = Volume × (High − Close) / (High − Low)
Delta       = Buy Volume − Sell Volume
```

This is an estimate based on OHLCV data, not actual bid/ask trade classification.

### Entry Conditions

**Long**

* Price closes above the opening range high.
* Opening range volume delta is positive.
* The distance between the entry price and opening range low is positive.

**Short**

* Price closes below the opening range low.
* Opening range volume delta is negative.
* The distance between the entry price and opening range high is positive.

### Risk Management

* Maximum one trade per day.
* Long stop loss: Opening range low.
* Short stop loss: Opening range high.
* Take profit: 1:1 risk-to-reward ratio.
* Backtesting starts with $100,000 in initial capital.

## Dataset

The backtest uses `df_1min.csv`, containing 1-minute market data.

The dataset covers:

* **Start:** January 3, 2010
* **End:** June 25, 2026

Expected columns:

`date`, `Open`, `High`, `Low`, `Close`, `Volume`

All timestamps are converted to the `America/New_York` timezone.

## Backtest Results

| Metric           |      ORB_15 |      ORB_30 |
| ---------------- | ----------: | ----------: |
| Initial Capital  |    $100,000 |    $100,000 |
| Final Equity     | $125,183.67 | $102,872.55 |
| Total Return     |      25.18% |       2.87% |
| Number of Trades |       3,321 |       3,013 |
| Win Rate         |      51.25% |      50.78% |
| Maximum Drawdown |     -20.93% |     -32.43% |
| Sharpe Ratio     |       0.160 |       0.016 |

*Historical backtest results are not indicative of future performance.*

## Requirements

* Python 3
* NumPy
* Pandas
* `backtesting.py`
* Jupyter Notebook

Install the required libraries:

```bash
pip install numpy pandas backtesting jupyter
```

## How to Run

1. Clone or download this repository.
2. Place `df_1min.csv` in the appropriate working directory.
3. Open the Jupyter Notebook.
4. Run the cells to calculate volume delta, execute both backtests, and review trade statistics.

## Disclaimer

This project is for research and educational purposes only. Backtest results may be affected by transaction costs, slippage, data quality, and execution assumptions. The strategies have not been demonstrated to guarantee future profitability.
