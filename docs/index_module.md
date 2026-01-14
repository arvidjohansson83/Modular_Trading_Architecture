# Index Module

## Overview

The index module is a standalone, single‑symbol execution system.  
Unlike the multi‑symbol forex architecture, each index EA is attached directly to the chart of the symbol it trades.  
This design reflects the unique behaviour of index markets, where volatility, session transitions, and structural ranges differ significantly from forex.

The module follows the same architectural principles as the rest of the framework:  
deterministic logic, clear state transitions, reproducible behaviour, and transparent logging.

---

## Entry Logic

The EA places pending orders at a predefined time based on the **premarket range**.  
This creates a consistent and repeatable entry structure:

- the premarket window defines the range  
- the EA calculates risk and order distance from that range  
- pending orders are placed at a fixed time  
- execution is isolated to the symbol on which the EA is running  

This ensures that each index EA behaves identically every day, without cross‑symbol dependencies.

---

## Reward Structure

Index markets exhibit a very different volatility profile compared to forex.  
Because of this, the module uses **multiple reward levels**, derived from long‑term statistical analysis.

The reward structure is split into two phases:

### 1. Premarket Reward
During premarket, volatility is lower and movements are more constrained.  
A lower reward level is used here, based on historically high hit‑rates for symbols such as:

- US30  
- DAX40  

### 2. Open Market Reward
When the market opens, volatility increases sharply.  
At this moment, the EA switches to a **higher reward level**, reflecting the increased probability of extended movement.

These reward levels are **weekday‑specific** and **symbol‑specific**, derived from multi‑year historical data.  
The goal is to select the reward level that historically represents the “most probable” or “best expected outcome” for that particular weekday and symbol.

---

## Trend Module Integration

The index module can optionally be combined with the **Trend Module**, using the same two‑position structure and extended logic that the trend system provides in the forex architecture.

When enabled, the EA splits the position into:

- **a TP‑leg**, targeting the statistically validated reward level for the day  
- **a Trend‑leg**, which follows market structure using the same trailing, fallback, and validation logic as the main Trend Module  

This allows index strategies to benefit from both the high hit‑rate reward levels and the potential for extended movement during high‑volatility sessions.

### Activation via Input Field

Trend functionality is controlled through a simple input field that defines, **weekday by weekday**, whether the trend strategy is allowed to activate.

This makes it possible to:

- enable trend behaviour only on days where historical data supports it  
- disable it on days where extended movement is less likely  
- maintain deterministic behaviour without modifying the code  

The integration is optional, lightweight, and fully consistent with the deterministic and modular design principles of the framework.

---

## Empirical Foundation

The entire reward system is based on statistical evidence.  
In my research, lower reward structures (e.g., 0.4:1) have shown very high hit‑rates on certain indices.  
This insight led to the development of a custom Python backtester to validate:

- alternative reward levels  
- weekday‑specific behaviour  
- volatility‑dependent edges  
- premarket vs open market dynamics  

The module is only expanded when a pattern repeats consistently across multiple years of data.

---

## Integration

Although index EAs are single‑symbol and chart‑attached, they still integrate with the shared components of the framework:

- logging  
- utilities  
- state management  
- deterministic execution patterns  

This ensures consistency across the entire system, even though index logic is isolated per symbol.
