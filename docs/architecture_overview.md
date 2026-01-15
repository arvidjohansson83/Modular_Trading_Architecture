# Architecture Overview

This document provides a high-level overview of the Modular Trading Architecture.  
The system is designed around clear separation of responsibilities, deterministic execution, and reproducibility across both live trading and backtesting environments.

---

## Core Components

### **1. Session Module**

Responsible for:
- Defining the premarket range  
- Placing pending orders at a scheduled timestamp  
- Calculating risk and position size per symbol  
- Managing multi-symbol execution through a single input field  

The session module acts as the entry point for all trades.

---

### **2. Trend Module**

Extends the session module by introducing:
- Two independent positions (TP-leg and trend-leg)  
- Split-risk allocation  
- Trend-following logic based on structural breaks  
- Fallback controls and ADX-based logging  

The trend module activates only after a session entry is triggered.

---

### **3. Backtesting Engine**

A standalone Python engine that:
- Retrieves historical data via the MT5 API or cached CSV files  
- Simulates the session and trend logic over arbitrary time periods  
- Performs reward sweeps from 0.4 to 10.0 with constant risk  
- Identifies stable long-term parameters per symbol  

The backtester mirrors the live logic to ensure reproducibility.

---

### **4. Shared Components**

Common utilities used across all modules:
- Risk model  
- Logging system  
- EMA data handling  
- Utility functions (time parsing, symbol handling, configuration parsing)  

---

### **5. Operational Safeguards**

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
3. **Shared Components** handle risk, logging, and utility functions.  
4. **Backtesting Engine** uses the same logic for historical simulation.  

---

## Repository Structure

```text
/src
/backtester         → Python backtesting engine
/session_module     → Session logic (premarket range, pending orders)
/trend_module       → Trend-following logic (TP-leg, trend-leg)
/shared             → Risk model, logging, utilities

/docs
architecture_overview.md
session_module.md
trend_module.md
backtester.md
risk_model.md
inputs_overview.md
logging_and_debugging.md
/examples           → Strategy examples, logs, walkthroughs
```

This structure ensures clarity, maintainability, and ease of navigation for both developers and reviewers.