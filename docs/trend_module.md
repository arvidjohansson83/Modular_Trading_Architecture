# Trend Module

The trend module manages directional bias, multi‑position handling, and trend‑following logic.  
It is designed for deterministic behavior, modularity, and transparent execution flow.

---

## System Behavior

The trend module activates only after a session entry has been triggered.  
Once active, it manages two independent positions:

- A **TP‑leg**, targeting a predefined take‑profit level  
- A **trend‑leg**, which remains open until a structural reversal occurs  

This dual‑leg structure allows the system to capture both **short‑term momentum** and **extended trend movement** within the same session.

The module continuously evaluates market structure to determine when the trend‑leg should remain active or be closed due to a reversal.

---

## Risk Allocation

The EA uses a **split‑risk model**, where total exposure is divided into two adjustable components:

- **TP‑leg risk**  
- **Trend‑following leg risk**

This separation ensures:

- predictable exposure  
- transparent risk distribution  
- independent behavior of each leg  
- consistent performance evaluation  

Each leg operates with its own risk profile while sharing the same directional bias.

---

## Technical Highlights

- **Session‑based activation** ensures the trend module only engages after a valid session entry.  
- **Fallback control** dynamically manages SL and TP levels as structure evolves.  
- **Trend‑leg management** is based on structural breaks rather than indicator‑based trailing.  
- **ADX logging** provides insight into trend strength at the moment of exit.  

These mechanisms ensure robust behavior in both trending and reversing environments.

---

## Architectural Principles

- **Modular EA design** with isolated trend logic  
- **Real‑time decision flow** based on market structure  
- **Transparent logging** for reproducibility and debugging  
- **Deterministic state transitions** for predictable behavior  

The trend module operates independently while integrating seamlessly with the session module, risk model, and logging system.

---

## Live Examples

For full live examples of the trend module in action, see the example library:

- **Trend Module – GBPJPY Example**  
  `docs/examples/trend_module_example_gbpjpy.md`

Additional examples will be added as the system evolves.

---

## Summary

The trend module provides a structured, deterministic approach to capturing both immediate and extended market movement.  
Its dual‑leg architecture, modular design, and transparent execution flow make it a reliable component within the overall trading framework.

For additional architecture diagrams and module breakdowns, see the main documentation in this repository.
