# Architecture Overview

This document provides a high-level overview of the Modular Trading Architecture.  
The system is designed around clear separation of responsibilities, deterministic execution, and reproducibility across both live trading and backtesting environments.

---

## Core Components

### **1. Session Module**

The session module is the foundational component of the architecture. It defines the deterministic structure that governs all entries and ensures identical behaviour across both live trading and backtesting. 

Key responsibilities: 
- **Premarket range computation** (high/low capture with fault‑tolerant logging) 
- **Scheduled pending‑order placement** at a fixed timestamp 
- **Risk and position‑size calculation** based on range geometry 
- **Multi‑symbol execution** through a single input field 
- **Deterministic state transitions** that eliminate timing‑based ambiguity 

Technical characteristics: 
- Uses locked premarket values to prevent indicator repainting 
- Ensures identical behaviour regardless of terminal latency or symbol load 
- Provides a unified entry structure for both forex and index modules 

The session module acts as the **entry point** for all trades and establishes the reproducible framework that the rest of the system builds upon.

---

### **2. Trend Module**

The trend module extends the session module with a structured, two‑position architecture designed to capture both immediate movement and extended trends. 

Core features: 
- **Two independent positions** (TP‑leg and trend‑leg) 
- **Split‑risk allocation** with deterministic sizing 
- **Structural trailing logic** based on break‑of‑structure (BOS) events 
- **5‑minute update cycle** for controlled, non‑reactive adjustments 
- **Fallback validation** to ensure safe state transitions 
- **ADX‑based contextual logging** (context only, not decision‑making) 

Technical characteristics: 
- Fully deterministic trailing behaviour 
- Symbol‑agnostic logic that adapts to volatility without curve‑fitting 
- Seamless integration with both forex and index modules 
- Designed to avoid noise‑driven exits and maintain structural integrity 

The trend module activates **only after a session entry is triggered**, and is engineered to capture extended movement when volatility supports it — without sacrificing reproducibility or control.

---

### **3. Index Module**

The index modules extend the architecture with symbol‑specific logic for high‑volatility markets such as **US30** and **DAX40**.  
They follow the same deterministic principles as the forex modules but introduce several behaviours tailored to index dynamics:

- **Multiple TP levels** for both pre‑market and open‑market sessions  
- **Reward‑based execution** driven by long‑term statistical patterns  
- **Volatility‑adaptive behaviour**, reflecting sharp transitions at market open  
- **Weekday‑specific and symbol‑specific hit‑rate profiles**  
- **Strict session alignment**, ensuring identical behaviour day‑to‑day  
- **Single‑symbol, chart‑attached execution**, avoiding cross‑symbol dependencies  

These modules were the first indication that certain markets support **higher reward levels** than the fixed 1:1 structure used in forex.  
Their behaviour revealed:

- stable low‑reward edges during pre‑market  
- extended movement potential during open‑market volatility  
- consistent multi‑year patterns across several indices  

This empirical foundation directly inspired the development of:

- the **Trend Module**, enabling extended movement capture  
- the **Python‑based Backtester**, designed to evaluate reward levels from **0.4 up to 10.0** with constant risk  

The index modules therefore serve as both a **practical trading component** and a **research driver** within the architecture.

---

### **4. Backtesting Engine**

A standalone Python engine that:
- Retrieves historical data via the MT5 API or cached CSV files  
- Simulates the session, index, and trend logic over arbitrary time periods  
- Performs reward sweeps from **0.4 to 10.0** with constant risk  
- Identifies stable long-term parameters per symbol  
- Mirrors live execution logic to ensure full reproducibility  

The backtester is designed to validate logic, not just outcomes — enabling deep inspection of state transitions, module interactions, and symbol-specific behaviour.

---

### **5. Shared Components**

Common utilities used across all modules:
- Risk model  
- Logging system  
- EMA data handling  
- Time and session utilities  
- Symbol configuration parsing  
- Deterministic state management  

These shared components ensure consistent behaviour across both live trading and backtesting environments.

---

### **6. Operational Safeguards**

To ensure robustness, fault tolerance, and consistent execution across both live trading and backtesting, the system includes several protective mechanisms:

- **Automatic cleanup of pending and active orders**  
  Ensures that no orphaned or unintended orders remain in the terminal after session completion or unexpected interruptions.

- **End‑of‑day statistical journal analysis**  
  Summarizes closed trades, providing daily performance metrics and validating that execution aligns with expected system behavior.

- **Premarket high/low logging for fault tolerance**  
  Stores the session range independently of MT5’s internal buffers, protecting against indicator repainting, terminal desynchronization, or data corruption.

- **Protective mechanisms against indicator repainting or terminal desync**  
  Ensures that all critical values (EMA, range, SL/TP levels) are captured and locked in real time, preventing retroactive changes or inconsistencies.

These safeguards reinforce the system’s reliability and operational integrity, especially during live trading conditions.

---

## Design Principles

### **Modularity**
Each module is isolated, testable, and replaceable without affecting the rest of the system.

### **Determinism**
Given the same inputs, the system produces the same outputs — both live and in backtests.

### **Reproducibility**
Logging, data handling, and execution logic follow consistent patterns across all environments.

### **Scalability**
Supports multi-symbol execution and can be extended with new modules without structural changes.

### **Transparency**
Risk models, parameters, and logic are fully documented and traceable.

---

## Data Flow

1. **Session Module** computes the premarket range and places pending orders.  
2. When an order is triggered, **Trend Module** (if enabled) takes over.  
3. **Index Modules** apply symbol‑specific reward logic where applicable.  
4. **Shared Components** handle risk, logging, and utility functions.  
5. **Backtesting Engine** uses the same logic for historical simulation.  

---

## Repository Structure

```text
/src
/backtester         → Python backtesting engine
/session_module     → Session logic (premarket range, pending orders)
/index_module       → Index-specific reward logic (multi-TP structure)
/trend_module       → Trend-following logic (TP-leg, trend-leg)
/shared             → Risk model, logging, utilities

/docs
architecture_overview.md
session_module.md
index_module.md
trend_module.md
backtester.md
risk_model.md
inputs_overview.md
logging_and_debugging.md

/docs/examples         → Strategy examples, logs, walkthroughs
```

This structure ensures clarity, maintainability, and ease of navigation for both developers and reviewers.