# FX Transaction Cost Analysis (TCA)

A transaction-cost analysis framework for FX order execution. It compares
execution algorithms on real intraday price data, decomposes execution cost
into its underlying drivers, and shows how the right algorithm depends on
order size.

## What it does

For thousands of hypothetical parent orders across three major currency
pairs, the framework:

1. Works each order under three execution algorithms — **Immediate** (trade
   all at once), **TWAP** (equal slices over time), and **Front-loaded**
   (larger slices early).
2. Applies a cost model to each child order: a fixed half-spread plus a
   square-root market-impact term.
3. Measures **implementation shortfall** — the gap between the average
   execution price and the arrival price — in basis points.
4. Decomposes that cost into **spread**, **market impact**, and **timing**
   components, and reports the standard deviation of shortfall as a measure
   of execution risk.

## What is real and what is modelled

- **Real:** intraday FX prices, pulled from Yahoo Finance.
- **Modelled:** the parent orders are hypothetical, and execution cost uses a
  stylised model — a fixed half-spread plus a square-root market-impact term,
  consistent with the market-impact literature. Desk TCA on a live platform
  would use actual fills and a venue-calibrated impact model.

The deliverable is the framework and the comparative analysis, not a profit
claim.

## Why it is built this way

- Faster execution removes exposure to price drift but pays more market
  impact; slicing reduces impact but increases timing risk. The framework
  quantifies that trade-off rather than asserting one algorithm is best.
- Cost is decomposed so the *driver* of slippage is visible, not just the
  total — the same question a trading desk asks in real TCA.
- Results are aggregated over thousands of executions, so conclusions are
  statistical rather than anecdotal.

## Running it

```bash
pip install yfinance pandas numpy matplotlib
python tca_fx_execution.py
```

The script prints the cost tables and saves a chart to `tca_results.png`.

## Results

*Run the script and paste the output tables here, alongside
`tca_results.png`.*

## Limitations and extensions

- No real fills or limit-order-book data; the impact coefficient is not
  calibrated to a specific venue.
- FX has no consolidated traded volume, so a VWAP benchmark is not used;
  arrival price is the benchmark.
- A natural extension is to split results by volatility regime, since timing
  cost should rise when markets move faster
