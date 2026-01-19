# Trend Module

The trend module manages directional bias, multi‑position handling, and trend‑following logic.  
It is designed for **deterministic behaviour**, **modularity**, and **transparent execution flow**, ensuring consistent results across both live trading and backtesting.

---

## System Behavior

The trend module activates **only after a valid session entry** has been triggered.  
Once active, it manages two fully independent positions:

- A **TP‑leg**, targeting a predefined, statistically validated take‑profit level  
- A **Trend‑leg**, which remains open until a structural reversal (BOS) occurs  

This dual‑leg structure allows the system to capture both:

- **short‑term momentum** (TP‑leg)  
- **extended directional movement** (trend‑leg)  

The module continuously evaluates market structure to determine whether the trend‑leg should remain active or be closed due to a confirmed reversal.

---

## Risk Allocation

The EA uses a **split‑risk model**, where total exposure is divided into two deterministic components:

- **TP‑leg risk**  
- **Trend‑leg risk**

This separation ensures:

- predictable exposure  
- transparent risk distribution  
- independent performance evaluation  
- consistent behaviour across symbols and sessions  

Each leg operates with its own risk profile while sharing the same directional bias derived from the session module.

---

## Technical Highlights

- **Session‑based activation**  
  The trend module only engages after a valid session entry, ensuring deterministic sequencing.

- **Structural trailing logic (BOS‑driven)**  
  The trend‑leg is managed using break‑of‑structure events rather than indicator‑based trailing, avoiding noise‑driven exits.

- **5‑minute update cycle**  
  Trailing and structural validation occur on a fixed interval, ensuring controlled, non‑reactive behaviour.

- **Fallback state machine**  
  SL and TP levels are validated and adjusted through a deterministic fallback mechanism that prevents invalid state transitions.

- **ADX contextual logging**  
  ADX values are logged at TP-leg exit to provide insight into trend strength, without influencing decision‑making.

These mechanisms ensure robust behaviour in both trending and reversing environments.

---

## Architectural Principles

- **Modular EA design**  
  Trend logic is isolated and can be enabled or disabled without affecting session behaviour.

- **Deterministic state transitions**  
  All decisions follow a predictable, reproducible flow based on structural rules.

- **Real‑time decision engine**  
  Market structure is evaluated continuously to determine trend‑leg validity.

- **Transparent logging**  
  Every structural event, trailing update, and fallback action is logged for reproducibility and debugging.

The trend module integrates seamlessly with the session module, risk model, and logging system while maintaining its own isolated logic.

---

## Live Examples

For full live examples of the trend module in action, see the example library:

- **Trend Module – GBPJPY Example**  
  `docs/examples/trend_module_example_gbpjpy.md`

Additional examples will be added as the system evolves.

---

## Summary

The trend module provides a structured, deterministic approach to capturing both immediate and extended market movement.  
Its dual‑leg architecture, BOS‑driven trailing, and transparent execution flow make it a reliable and scalable component within the overall trading framework.

For additional architecture diagrams and module breakdowns, see the main documentation in this repository.
