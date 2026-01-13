Backtester – Live Input and Trade Loop Execution (Video)
Video captured: 2026‑01‑XX
Context: Manual input demonstration and day‑by‑day trade evaluation
Description: Deterministic USD‑based backtesting with transparent trade‑by‑trade output

The following video demonstrates how the Python backtester processes trades using the same fixed‑risk model as the live EA.
In this example, inputs are entered manually, and the backtester iterates through each trading day, evaluating trades in pure USD terms.

The video illustrates:

how user inputs (risk, RR, session times, symbol settings) are entered

how the backtester loads historical data and prepares the session windows

how each day is processed sequentially

how SL/TP distances are evaluated in pips

how RiskPerTradeUSD is applied directly without MT5 lot‑size logic

how each trade’s result is written to the output structure in real time

how the final dataset is built deterministically from the trade loop

This example highlights the transparency of the backtesting engine and demonstrates how the system reproduces the EA’s behavior using a clean, platform‑independent calculation model.