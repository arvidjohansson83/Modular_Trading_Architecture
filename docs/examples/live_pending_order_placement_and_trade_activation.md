# Live Pending Order Placement and Trade Activation (Video)

**Video captured:** 2026‑01‑12
**Symbols:** Multiple (from Daily Trade List)
**Context:** Live session timestamp execution
**Description:** Automatic placement of pending orders with deterministic 1:1 RR

[▶️ Watch the video on YouTube](https://youtube.com/shorts/jnR44ETgoQU?si=elL8LvxRfz8N1O_V)

The following video demonstrates how the Session Module executes pending order placement based on the Daily Trade List input field. All pending orders for all symbols are placed simultaneously at the session timestamp.

The video illustrates:

- how the EA reads the Daily Trade List configuration
- how each symbol receives the correct pending order type (STOP or LIMIT)
- how RiskPerTradeUSD is applied from its dedicated input field
- how Entry, SL, and TP levels are calculated directly from the premarket range
- how the resulting Risk‑to‑Reward ratio is exactly 1:1 for all trades

Three of the pending orders are also triggered during the recording, providing a real‑time demonstration of how the EA transitions from order placement to active trade management with a precise 1:1 risk‑to‑reward structure.

This visual example confirms the deterministic nature of the system and demonstrates how the EA translates user inputs into precise, reproducible execution.