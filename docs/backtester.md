# Backtesting Engine

The backtesting engine is a standalone Python component designed to reproduce the system’s behavior with full determinism.
It simulates the EA’s logic bar‑by‑bar using historical data retrieved via the MT5 API or cached CSV files, mirroring live execution across:

- session logic
- EMA‑based directional filters
- order type selection (STOP/LIMIT)
- strategy‑specific SL/TP rules
- fixed‑risk position sizing

The goal is to provide a transparent, reproducible environment for validating system behavior, discovering statistical edge, and identifying stable long‑term parameters per symbol.

---

## Features

### Historical Data Retrieval

The engine supports three modes of data loading:

1. **Full history mode**  
   - Automatically loads all data that is avaiable in the terminal 
   - Start and end dates are generated dynamically  

2. **Manual candle count**  
   - User specifies number of bars  
   - User provides start and end dates  

3. **Cached CSV mode**  
   - Loads pre‑saved historical data from disk  
   - User specifies date range to filter  

Configurable parameters:

- Symbols to backtest (comma‑separated list)  
- Timeframe (M5, M15, M30, H1, H4)  
- Start and end dates (YYYY-MM-DD)
- Number of candles (optional)  
- EMA fast/slow periods (a condition)  
- Path to `Global_EMA_log.csv` where EMA calculations have been made.

---

## Session Logic Simulation

The backtester replicates the EA’s session behavior:

- Computes the premarket range using user‑defined session start and end times  
- Applies a **cutoff time** for forced trade closure (to avoid daily swaps)
- Evaluates EMA conditions at the end of the user-defined session  
- User selects between STOP or LIMIT order type to backtest
- Applies the same directional logic as the EA:

´´´text
STOP + EMA_fast > EMA_slow → Buy Stop
STOP + EMA_fast < EMA_slow → Sell Stop
LIMIT + EMA_fast > EMA_slow → Sell Limit
LIMIT + EMA_fast < EMA_slow → Buy Limit
´´´

The engine then simulates order execution bar‑by‑bar using historical OHLC data.

---

## Strategy Logic

Currently - the backtester supports two strategies:

### 1. **Premarket Strategy**
- Direction determined by EMA relationship  
- SL level is **automatically assigned** based on order type and direction  
- TP levels are selected from predefined Fibonacci‑based offsets  
- Cutoff time enforces session exit  

### 2. **Breakout Strategy**
- User selects SL level manually from a list of Fibonacci offsets  
- TP levels are configurable  
- Direction still determined by EMA relationship  

---

## Risk Model

Risk is defined **per trade in USD**, identical to the EA: 

´´´text
RiskPerTradeUSD = X
This fixed‑risk model applies to **both strategies**, ensuring consistent exposure across all backtests.
´´´

Lot size is computed using:

```python
LotSize = RiskPerTradeUSD / (RangeInPips * PipValue)
```
This ensures:

- deterministic sizing
- identical behavior between backtest and live trading
- consistent exposure across symbols

---

## Reward Sweep

The engine performs a reward sweep across a range of TP multipliers from 0.4 → 10.0

Risk remains constant to isolate the effect of reward parameters.
This enables discovery of stable long‑term reward levels per symbol and per strategy — a key component of **statistical edge discovery**.

---

## Weekday Filtering

The user may restrict backtests to:

- all days
- only Mondays
- only Tuesdays
- only Wednesdays
- only Thursdays
- only Fridays

This enables weekday‑specific statistical profiling.

---

## Output

The backtester produces:

- performance metrics per reward level
- symbol‑by‑symbol comparisons
- EMA‑aligned directional statistics
- SL/TP hit distribution
- identification of stable long‑term parameters
- logs for debugging and reproducibility

---

## Design Goals

- Reproducibility
- Identical logic between live trading and backtesting
- Data‑driven parameter selection
- Transparent and deterministic execution