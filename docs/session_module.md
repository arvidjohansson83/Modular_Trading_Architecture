# Session Module

The session module is responsible for all initial trade execution.  
It is designed for **deterministic behaviour**, **minimal configuration**, and **scalable multi‑symbol operation**.

---

## Responsibilities

### Premarket Range Calculation
The module computes the premarket high/low range based on **hard‑coded time values** inside the code.  
These times are **not user‑configurable** through input fields in the current version.

The premarket range is used for:

- pending order placement  
- lot size calculation  
- risk allocation  

---

## Pending Order Placement

### Live Example (Video)  
[▶️ Watch Live Pending Order Placement and Trade Activation](https://youtube.com/shorts/jnR44ETgoQU?si=elL8LvxRfz8N1O_V)

This video demonstrates how the Session Module places all pending orders simultaneously at the session timestamp based on the Daily Trade List input field.  
It also shows three of the pending orders being triggered during the recording, providing a real‑time example of how the EA transitions from order placement to active trade management with a precise 1:1 risk‑to‑reward structure.

The module supports all four pending order types:

- **Buy Stop**  
- **Sell Stop**  
- **Buy Limit**  
- **Sell Limit**

Which order type to use is controlled via a single input field in the format:

SYMBOL:ORDERTYPE;

Examples:

XAUUSD:STOP;
NAS100:LIMIT;


This allows the system to run multiple symbols with different order behaviours using a single configuration string.

---

### Order Logic

At the moment the premarket session ends, the system evaluates the EMA conditions and selects the correct pending order automatically:

- If `OrderType = STOP`  and `EMA_fast > EMA_slow` → **Buy Stop**  
- If `OrderType = STOP`  and `EMA_fast < EMA_slow` → **Sell Stop**  
- If `OrderType = LIMIT` and `EMA_fast > EMA_slow` → **Sell Limit**  
- If `OrderType = LIMIT` and `EMA_fast < EMA_slow` → **Buy Limit**  

This ensures **deterministic behaviour** without requiring the user to manually select the specific pending order.

---

### Why LIMIT Orders Matter

LIMIT orders enable the system to participate in **reversal‑type setups** that often occur immediately after the premarket range is formed.

This provides:

- the ability to capture **pullbacks** into the range  
- entries at **better prices** than STOP orders  
- exposure to **mean‑reversion behaviour**  
- a complementary execution style to breakout continuation  

STOP orders target **momentum continuation**.  
LIMIT orders target **mean‑reversion entries**.

Both are driven by the same **EMA‑based directional bias**, ensuring consistent logic across execution styles.

---

## Risk and Position Sizing

Risk is defined **per trade in USD**, via a user input field:

RiskPerTradeUSD = X

There is **no percentage‑based risk model**.

### Workflow

1. The user determines the **total daily risk** they want to allocate.  
2. The user divides this across the number of trades they intend to place.  
3. The EA uses `RiskPerTradeUSD` to compute lot size.  
4. Lot size is calculated **only** from the premarket range distance (in pips):

```python
LotSize = RiskPerTradeUSD / (RangeInPips * PipValue)
```

This makes the system:

- deterministic  
- transparent  
- easy to validate across symbols  

---

## Multi‑Symbol Execution

A single input field controls which symbols are traded.  
The field accepts any number of symbols, separated by semicolons: 

EURUSD:STOP;BTCUSD:LIMIT;UK100:STOP;

…with **no change in logic**.

There is **no upper limit** on the number of symbols.
Each symbol runs its own independent:

- premarket calculation
- pending order logic
- risk model

This design makes the module fully scalable from **1 symbol to dozens** of symbols without any change in logic.

Configuration, daily symbol selection, and dashboard visualization are
documented separately here:

[Operational Interface – Inputs & Dashboard](operational_interface.md)

---

## Reward Logic

- **Forex:** fixed RR of 1:1  
- **Indices:** adjustable RR from 0.4 upward  
- **Indices:** two TP levels (premarket TP and open‑market TP)  

---

## Design Goals

- Deterministic execution  
- Minimal configuration  
- Clear and predictable risk exposure  
- Scalable multi‑symbol operation  
- Clean separation between session logic, trend logic, and risk logic  
