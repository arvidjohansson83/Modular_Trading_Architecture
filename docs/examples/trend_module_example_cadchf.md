# Trend Module – CADCHF Example

**Image captured:** 2025‑12‑05  
**Symbol:** CADCHF  
**Context:** Live execution example from the forex module with trend module enabled  
**Description:** Two‑position logic with TP‑leg and trend‑leg behavior following macro event breakout

![Trend Module Example – CADCHF](IMG_20251205_trend_example_cadchf.jpg)

_The EA executed a predefined entry and take‑profit level following a strong Canadian employment report.  
Price reached the TP during the initial impulse, and the trend‑leg remained active until market close Friday evening._

---

## 🧠 What Happened in This Specific Trade

At the scheduled session timestamp, the EA opened **two buy positions**:

- The **TP‑leg** reached its take‑profit level cleanly as price accelerated upward.  
- The **trend‑leg** remained open and was managed by the trend module’s structural trailing logic.  
- The position was held until **market close Friday evening**, with multiple trailing adjustments throughout the day.

This trade shows how the system captures both the **initial impulse** and manages **extended continuation** with disciplined stop logic.

---

## 🔄 5‑Minute Structural Update Cycle and Stop‑Loss Adjustment

The trend‑leg was evaluated on the system’s **fixed five‑minute update cycle**.  
During each evaluation, the module determined whether the prevailing structure justified advancing the protective stop.

In this trade:

- Strong momentum resulted in **multiple upward SL adjustments** (buy trade).  
- Structure remained intact throughout the day, allowing the position to remain open.  
- The final exit occurred at **market close**, locking in the full structural move.

This controlled, non‑reactive trailing behavior ensures:

- profit protection  
- trend‑following logic  
- no premature exits due to noise  
- deterministic, rule‑based execution

---

## ⚙️ Fallback Control in Action

The fallback system ensures that:

- TP levels are stored safely  
- SL adjustments are validated  
- no trailing step is applied unless conditions are met  
- the system remains deterministic even during volatility

In this trade, fallback control confirmed:

- the saved TP level  
- the correct SL movement  
- that the SL update was valid (`SLMoved=true`)  
- that the trend‑leg remained active until structural exit

---

## 📋 Log Extract (translated)

```text
TP/SL logic for CADCHF  
TP progress for trend position: 406.77%  
Fallback control: savedTPLevel=0.57726, SL=0.58046, Entry=0.57593, SLMoved=true
```
*Note: TP progress reflects the distance from the TP‑leg’s predefined TP level — this is how the trend module quantifies extended structural continuation.*

---

## 📌 Summary of This Trade

This CADCHF buy trade demonstrates the trend module’s live behavior:

- **TP‑leg** captures the initial directional impulse  
- **Trend‑leg** follows the move using structural trailing  
- **SL is evaluated every 5 minutes**  
- **Trailing logic adjusts SL upward only when justified**  
- **TP progress exceeded 400%**, showing extended continuation  
- **Exit occurred at market close**, not on indicator noise  

This example shows exactly how the trend module behaves in real market conditions:  
**disciplined, deterministic, and structurally driven.**
