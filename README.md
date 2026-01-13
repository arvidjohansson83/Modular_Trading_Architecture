# Modular Trading Framework

A deterministic, multi-symbol trading architecture built around clean module boundaries, reproducible execution, and transparent state management.  
This repository contains both the live MQL5 implementation and the supporting Python backtesting engine.

---

## 🚀 Purpose

This project demonstrates how to design a scalable, fault‑tolerant trading system using:

- strict modular separation  
- deterministic state transitions  
- reproducible logic across live trading and backtesting  
- transparent logging and operational safeguards  

The goal is to provide a clear, professional architecture suitable for systematic trading, research, and portfolio‑grade engineering.

---

## 📐 High-Level Architecture

The system is composed of several independent modules, each responsible for a specific part of the execution pipeline.  
A full breakdown of each module is available in:

/docs/architecture_overview.md

This document explains:

- how the modules interact  
- how data flows through the system  
- how session logic, trend logic, and shared utilities integrate  
- how operational safeguards ensure stability  

The README focuses on navigation and usage rather than repeating those details.

---

## 📁 Repository Structure

/src
main.mq5
/session_module
/trend_module
/shared
/utils

/backtester
python engine for historical simulation

/docs
architecture_overview.md
session_module.md
trend_module.md
backtester.md
risk_model.md
inputs_overview.md
logging_and_debugging.md

/docs/examples
trend_module_execution_gbpjpy.md

Each document is self‑contained and focuses on a single part of the system.

---

## 📊 Example Executions

Live examples are included to illustrate how the system behaves under real market conditions.

### Example: GBPJPY Trend Execution  
Demonstrates:

- two‑position entry structure  
- structural trailing  
- fallback validation  
- deterministic SL adjustments  
- ADX context logging  

See:  
`/docs/examples/trend_module_execution_gbpjpy.md`

Additional examples will be added over time.

---

## 🧠 Design Philosophy

The framework is built around:

- **Determinism** — identical inputs produce identical outputs  
- **Modularity** — each component is isolated and testable  
- **Reproducibility** — live and backtest logic share the same structure  
- **Transparency** — all decisions are logged and traceable  
- **Scalability** — multi‑symbol execution without code duplication  

These principles guide both the MQL5 implementation and the Python backtester.

---

## 🛠️ Technologies

- **MQL5** for live execution  
- **Python** for backtesting and analysis  
- **Git** for version control  
- **VS Code** for development  
- **MT5 API** for data retrieval  

---

## 📌 Notes

This project is intended for educational and portfolio purposes.  
It is not a recommendation to trade and should not be used live without proper validation.

