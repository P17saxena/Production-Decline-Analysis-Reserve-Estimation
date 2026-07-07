Production Decline Analysis & Reserves Estimation
A Python tool that fits Arps decline models (exponential, harmonic, hyperbolic) to well production data, statistically selects the best-fitting model, forecasts EUR (Estimated Ultimate Recovery), and cross-checks that result against an independent volumetric OOIP estimate using Monte Carlo simulation.
Why this project
Reservoir engineers routinely reconcile two independent estimates of a well's reserves:
Production-based (decline curve analysis) — extrapolating a well's historical rate decline forward to an economic limit.
Static/volumetric (rock & fluid properties) — estimating oil-in-place from area, thickness, porosity, and saturation, independent of any production history.
Agreement between the two builds confidence in the reserves estimate; disagreement flags a problem with an assumption somewhere. This project reproduces that full reconciliation workflow end-to-end.
Methodology
Synthetic well data — generated using realistic Arps parameters for a moderate-rate onshore well, with random noise added to simulate real measurement scatter. Using synthetic data with known "true" parameters lets the fitting code be validated (does it recover the true parameters?) before being trusted on real field data.
Curve fitting — exponential, harmonic, and hyperbolic Arps models are each fit to the data via `scipy.optimize.curve_fit`.
Model selection — R² and AIC are computed for each fit; AIC is used as the actual selection criterion since it penalizes the hyperbolic model's extra parameter, avoiding a bias toward overfitting.
EUR forecast — the best-fit model is projected forward to an assumed economic limit rate, and integrated to estimate total recoverable volume.
Volumetric OOIP (Monte Carlo) — 10,000 simulations sampling realistic ranges for area, net pay thickness, porosity, and water saturation, producing a full OOIP distribution (P10/P50/P90).
Recovery factor check — EUR divided by OOIP (P50), compared against typical primary-recovery benchmarks as a sanity check on both estimates.
Results (this run)
Metric	Value
Best-fit model	Hyperbolic
EUR	~1,298,000 bbl
OOIP (P50)	~16,378,000 bbl
Recovery Factor	~7.9%
(Note: since this uses a fixed random seed, rerunning `DCA_Reserves_Estimation... .ipynb` will reproduce these exact numbers.)
Repository contents
`Decline_Curve_Analysis.ipynb` — the full analysis notebook
`requirements.txt` — Python dependencies
How to run
```bash
pip install -r requirements.txt
jupyter notebook Decline_Curve_Analysis.ipynb
```
Then: `Kernel → Restart Kernel and Run All Cells`.
Limitations & possible extensions
Uses synthetic data — swapping in real public well production data (e.g. Texas RRC, North Dakota DMR) is a natural next step.
Oil formation volume factor (Boi) is held fixed rather than sampled as a distribution.
EUR is sensitive to the assumed economic limit rate; a sensitivity sweep would strengthen this further.
A tornado chart isolating which volumetric input drives the most OOIP uncertainty would be a good addition.

