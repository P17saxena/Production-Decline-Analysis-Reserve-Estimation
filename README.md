# Production Decline Analysis & Reserves Estimation

A Python-based engineering tool that fits Arps decline models to production data, statistically selects the optimal forecast model to estimate **EUR (Estimated Ultimate Recovery)**, and cross-checks the results against an independent **volumetric OOIP (Original Oil in Place)** estimate via Monte Carlo simulation.

---

## Project Overview & Engineering Context

In reservoir engineering, reconciling independent reserve estimates is a critical workflow. This tool automates the validation loop between two classic methodologies:

1. **Dynamic/Production-Based (DCA):** Extrapolates historical flow rates forward to a predefined economic limit using Arps decline equations.
2. **Static/Volumetric (Rock & Fluid Properties):** Estimates total oil-in-place based on subsurface parameters (area, net pay, porosity, water saturation) independent of production history.

> **Why Reconcile?** Agreement between both methods builds high confidence in asset valuation. Significant discrepancies flag fundamental issues with either geological assumptions or production constraints. This project reproduces that end-to-end reconciliation workflow.

   
