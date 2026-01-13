# Logging and Debugging

The system uses a transparent logging model to ensure reproducibility and traceability across both live trading and backtesting.

---

## Logging Principles

- Every decision point is logged  
- EMA values are recorded from `Global_EMA_log.csv`  
- SL/TP adjustments are tracked  
- Trend module logs ADX values at exit  
- Backtester logs session boundaries and triggered events  

---

## Log Types

### **Session Logs**
- Premarket range  
- Order placement  
- EMA direction  
- Cutoff enforcement  

### **Trend Logs**
- Structural break detection  
- SL movement  
- ADX at exit  
- TP-leg and trend-leg outcomes  

### **Backtester Logs**
- Reward sweep results  
- Weekday filtering  
- Strategy selection  
- Execution timeline  

---

## Debugging Workflow

1. Validate EMA CSV path  
2. Confirm session window  
3. Check order type and strategy  
4. Inspect SL/TP logic  
5. Review backtester logs for anomalies  

---

## Example Log Snippet

Fallback control: savedTPLevel=207.990, SL=207.318, Entry=208.189, SLMoved=true
ADX value after TP exit: 33.51