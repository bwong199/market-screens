# IBS mean-reversion backtest: Dow 30 and TSX 60

Market data research only; not investment advice. Written 2026-11-27.

## Rule

IBS (internal bar strength) = (Close - Low) / (High - Low) of the daily bar.

- **Entry:** buy at the close when IBS < x.
- **Exit:** sell at the close when IBS > y.
- Long only, one position per stock, 0.05% cost per side, adjusted prices from Yahoo Finance.
- Portfolio: capital split equally across the stocks, cash idle when a stock is flat. Benchmark: the same stocks, equal weight, rebalanced daily.
- Sharpe = mean / stdev of daily returns x sqrt(252), risk-free rate 0. Calmar = CAGR / worst drawdown.
- Grid: x in {0.05, 0.10, 0.15, 0.20, 0.30, 0.50}, y in {0.7, 0.8, 0.9, 0.95}. Results were also split into a first and a second half of the period to check that the edge was not just in-sample.

## Universe and period

| Test | Period | Dow 30 | TSX 60 |
|---|---|---|---|
| 10 years | 2016-11-28 to 2026-11-27 | 30 stocks | 59 stocks |
| 20 years | 2006-11-27 to 2026-11-27 | 30 stocks | 57 stocks |

Current index members are used, so stocks that left the index are missing (survivorship bias). The 20-year TSX test drops names with fewer than 2,500 daily bars.

## Results: 20 years, Dow 30

| Strategy | Sharpe | Calmar | CAGR | Max drawdown | Sharpe / Calmar, 1st half | Sharpe / Calmar, 2nd half |
|---|---|---|---|---|---|---|
| Buy and hold | 0.89 | 0.37 | 16.2% | -43.7% | 0.78 / 0.34 | 1.02 / 0.54 |
| x<0.05, y>0.9 | 1.15 | 0.81 | 9.1% | -11.2% | 1.21 / 1.01 | 1.10 / 0.69 |
| x<0.10, y>0.9 | 1.15 | 0.71 | 12.6% | -17.8% | 1.07 / 0.74 | 1.26 / 1.09 |
| x<0.10, y>0.95 | 1.17 | 0.77 | 15.4% | -20.0% | 1.05 / 0.76 | 1.33 / 1.24 |
| x<0.20, y>0.9 | 1.08 | 0.76 | 14.6% | -19.3% | 1.01 / 0.77 | 1.18 / 1.05 |
| x<0.30, y>0.9 | 1.03 | 0.75 | 15.2% | -20.3% | 1.00 / 0.79 | 1.08 / 0.87 |
| x<0.50, y>0.9 | 0.98 | 0.70 | 15.6% | -22.3% | 0.96 / 0.74 | 1.03 / 0.85 |

## Results: 20 years, TSX 60

| Strategy | Sharpe | Calmar | CAGR | Max drawdown | Sharpe / Calmar, 1st half | Sharpe / Calmar, 2nd half |
|---|---|---|---|---|---|---|
| Buy and hold | 1.05 | 0.42 | 15.4% | -37.0% | 0.95 / 0.44 | 1.15 / 0.46 |
| x<0.05, y>0.7 | 1.37 | 0.75 | 4.9% | -6.5% | 1.86 / 1.64 | 0.85 / 0.45 |
| x<0.05, y>0.8 | 1.38 | 0.67 | 6.1% | -9.0% | 1.76 / 1.44 | 0.99 / 0.46 |
| x<0.10, y>0.9 | 1.28 | 0.67 | 10.6% | -15.7% | 1.40 / 0.92 | 1.16 / 0.61 |
| x<0.10, y>0.95 | 1.28 | 0.64 | 12.8% | -20.1% | 1.38 / 0.88 | 1.19 / 0.61 |
| x<0.20, y>0.9 | 1.21 | 0.61 | 12.8% | -20.9% | 1.30 / 1.01 | 1.14 / 0.59 |
| x<0.20, y>0.95 | 1.24 | 0.62 | 14.8% | -23.8% | 1.30 / 0.90 | 1.19 / 0.61 |
| x<0.50, y>0.9 | 1.08 | 0.48 | 13.2% | -27.3% | 1.12 / 0.84 | 1.04 / 0.48 |

## Results: 10 years (earlier run, entry cost on exits only)

| Market | Strategy | Sharpe | Calmar | CAGR | Max drawdown |
|---|---|---|---|---|---|
| Dow 30 | Buy and hold | 1.02 | 0.53 | 17.6% | -32.8% |
| Dow 30 | x<0.10, y>0.9 | 1.35 | 1.19 | 13.0% | -10.9% |
| TSX 60 | Buy and hold | 1.14 | 0.47 | 16.9% | -36.4% |
| TSX 60 | x<0.10, y>0.9 | 1.24 | 0.66 | 10.3% | -15.6% |

## Findings

- A low entry (IBS below 0.05 to 0.10) and a high exit (above 0.9 to 0.95) give the best risk-adjusted results on both markets. Raising the entry cutoff toward 0.5 steadily lowers Sharpe and Calmar.
- Every setting at or below 0.20 beats buy-and-hold on Sharpe and Calmar over 20 years in both markets. The advantage is mostly lower drawdown (about -10% to -20% against -37% to -44%), not higher return.
- The strategy earns less than buy-and-hold in total return at the strictest entries, because it holds cash much of the time. Looser entries (below 0.20 to 0.30) with exit above 0.9 get close to buy-and-hold's CAGR with about half the drawdown.
- The edge fades. On the TSX 60 the first half scored Sharpe 1.4 to 1.9 and the second half 0.8 to 1.2, which only matches buy-and-hold (1.15). The Dow 30 held up better in the second half (Sharpe 1.26 to 1.33 at x<0.10, y>0.9 to 0.95).

## Caveats

- Survivorship bias from using today's index members.
- Fills are assumed at the signal-day close, which is optimistic.
- The grid has 24 cells, and the best-looking cell in one half is often not the best in the other half. Read the region, not one cell.
- No stops, position limits, taxes or financing. Cash earns nothing in the model.
- The 20-year window includes 2008-2009, which strongly rewards a strategy that holds cash and buys washed-out closes.

## Reproduce

`.tools/ibs_backtest/backtest_20y.py` (Sharpe / Calmar grid) and `.tools/ibs_backtest/grid_per_day_10y.py` (return per invested day) in the private PersonalTrading project. Both use yfinance and the iShares TSX 60 holdings file.
