# Risk Model

The risk model defines how exposure is controlled across trades and modules.  
The system uses a fixed USD‑based risk model, ensuring deterministic and transparent behavior across both live trading and backtesting.

---

## Core Principles

### **Fixed USD Risk Per Trade**
Risk is defined explicitly by the user:

RiskPerTradeUSD = X

There is **no percentage‑based risk model** and no dependency on account balance.  
The user determines their total daily risk budget and divides it across the number of trades they intend to place.

---

## Position Sizing (Live EA)

The EA converts `RiskPerTradeUSD` into a lot size using only:

- the premarket range (in pips)  
- pip value for the symbol  

Lot size is computed as:

```python
LotSize = RiskPerTradeUSD / (RangeInPips * PipValue)
```
This makes the system:

- deterministic
- transparent
- easy to validate across symbols
- independent of account balance

No volatility filters, ATR‑based sizing, or percentage‑based scaling are used.

---

## Position Sizing (Backtester)

The Python backtester does not compute lot size.
Instead, it evaluates trades directly in USD using:

- RiskPerTradeUSD
- SL/TP distances in pips
- reward multipliers

This removes all MT5‑specific sizing logic and ensures clean, reproducible simulations.

---

## Split‑Risk Allocation (Trend Module)

When the trend module is active, the fixed USD risk is divided into two independent legs:

- TP‑leg (short‑term exit logic)
- Trend‑leg (structural trend logic)

Each leg has its own stop‑loss, exit conditions, and logging.

---

## Multi‑Symbol Execution

The system does not allocate risk automatically per symbol.

Instead:

- the user defines a fixed USD risk per trade
- this value is applied identically to every symbol
- the user is responsible for distributing their daily risk budget across symbols and trades

This keeps the model simple, predictable, and fully under user control.

---

## Design Goals

- Deterministic exposure
- Transparent and predictable sizing
- No dependency on account balance
- Identical risk definition across EA and backtester
- Easy validation across symbols and strategies