# Production Decline Analysis & Reserves Estimation

A Python-based engineering tool that fits Arps decline models to production data, statistically selects the optimal forecast model to estimate **EUR (Estimated Ultimate Recovery)**, and cross-checks the results against an independent **volumetric OOIP (Original Oil in Place)** estimate via Monte Carlo simulation.

---

## 📌 Project Overview & Engineering Context

In reservoir engineering, reconciling independent reserve estimates is a critical workflow. This tool automates the validation loop between two classic methodologies:

1. **Dynamic/Production-Based (DCA):** Extrapolates historical flow rates forward to a predefined economic limit using Arps decline equations.
2. **Static/Volumetric (Rock & Fluid Properties):** Estimates total oil-in-place based on subsurface parameters (area, net pay, porosity, water saturation) independent of production history.

> **Why Reconcile?** Agreement between both methods builds high confidence in asset valuation. Significant discrepancies flag fundamental issues with either geological assumptions or production constraints. This project reproduces that end-to-end reconciliation workflow.

---

## ⚙️ Methodology & Workflow

### 1. Synthetic Data Generation
* Generates a realistic production history for a moderate-rate onshore well.
* Introduces random Gaussian noise to simulate real-world measurement scatter and multi-phase flow fluctuations.
* *Value:* Using a known synthetic baseline allows us to validate whether the regression algorithms can accurately recover the true underlying decline parameters before deploying the model on messy field data.

### 2. Curve Fitting & Statistical Model Selection
* Fits three distinct Arps decline models (Exponential, Harmonic, and Hyperbolic) to the data using non-linear least squares (`scipy.optimize.curve_fit`).
* Evaluates goodness-of-fit using $R^2$ and **AIC (Akaike Information Criterion)**. 
* *Note:* AIC is utilized as the primary selection metric because it penalizes the hyperbolic model for its extra degree of freedom (the $b$-exponent), effectively mitigating overfitting risks.

### 3. EUR Forecasting
* Projects the statistically superior model forward until it intersects an assumed economic limit rate.
* Integrates the rate-time curve over the remaining lifespan to compute cumulative production and final **EUR**.

### 4. Volumetric OOIP (Monte Carlo Simulation)
* Runs 10,000 stochastic iterations, sampling realistic statistical distributions for drainage area ($A$), net pay thickness ($h$), porosity ($\phi$), and water saturation ($S_w$).
* Outputs a full probabilistic distribution of OOIP, isolating the **P10, P50, and P90** uncertainty thresholds.

### 5. Recovery Factor (RF) Sanity Check
* Divides the calculated DCA EUR by the volumetric P50 OOIP.
* Compares the resulting RF against typical regional primary-recovery benchmarks to serve as a final diagnostic sanity check.

---

## 📊 Run Results

Because this workflow utilizes a fixed random seed, executing the notebook will consistently reproduce the following deterministic baseline results:

| Reservoir Metric | Evaluated Value | Notes / Selection Criteria |
| :--- | :--- | :--- |
| **Best-Fit Decline Model** | **Hyperbolic** | Selected via lowest AIC score |
| **Estimated Ultimate Recovery (EUR)** | **~1,298,000 bbl** | Integrated to economic limit |
| **Volumetric OOIP (P50)** | **~16,378,000 bbl** | Median value from 10k Monte Carlo runs |
| **Calculated Recovery Factor (RF)** | **~7.9%** | Matches typical primary recovery windows |

---

## 📂 Repository Contents

* `Decline_Curve_Analysis.ipynb` — The complete, self-contained Jupyter notebook containing data generation, mathematical regressions, Monte Carlo simulations, and data visualizations.
* `requirements.txt` — Python package dependencies required to spin up the environment.

---

## 🚀 Getting Started & Execution

### Prerequisites
Ensure you have Python 3.x installed on your machine.

### Installation
1. Clone this repository to your local machine.
2. Open your terminal or command prompt in the project root directory and install dependencies:
   ```bash
   pip install -r requirements.txt
   
