# Bi/Tri Weibull & Crow-AMSAA Reliability Analysis Toolkit

A free, macro-enabled Excel workbook for reliability engineering analysis — Weibull/Exponential/Lognormal life data analysis, Weibayes zero-failure analysis and demonstration planning, Crow-AMSAA (NHPP power-law) reliability growth analysis, Fleet MCF comparison of repairable units, non-parametric survival analysis, and plant-level maintenance cost forecasting — with full confidence-bound calculations, built entirely in native Excel + VBA. No add-ins, no external software required.

## What it does

### Life data analysis (Weibull / Exponential / Lognormal)

- 2-parameter and 3-parameter ("Tri") Weibull fitting, plus Exponential and Lognormal fits for side-by-side distribution comparison — all available across every data-entry path (individual records, grouped data, and grouped interval-inspection data)
- Rank regression and Maximum Likelihood Estimation (MLE), with a reduced-bias (RBA) MLE Beta for small samples
- Supports suspended/censored data, grouped data, and grouped interval-inspection data
- Confidence bounds: Fisher Matrix, Likelihood Ratio, Bayesian credible bounds (with your own prior), and parametric bootstrap bounds
- Goodness-of-fit reporting (Anderson-Darling AD*, Stephens-adjusted, for individual data; G² deviance and AIC for grouped and inspection data) and a Distribution Fit Comparison table to help you pick the best-fitting model
- Spares forecasting and cost analysis built on the fitted distribution

### Weibayes / zero-failure analysis

- Answers "we've had no failures (or very few) — what life can we claim?" from the running time of every unit and an assumed or historical Beta
- Available on every data-entry path from the analysis choice form. It uses the Failure/Suspension flags exactly as they are on the input sheet, so mark units still running as S before you start
- One-sided lower bounds on Eta and on the B-life (e.g. B10), reliability at a mission time, and an upper bound and Weibayes estimate when there are failures
- Demonstration planning: shows whether a target B-life is already demonstrated, how much more failure-free running each unit needs, or how long new test units must run — with a What-If form that suggests realistic targets from your data
- Beta sensitivity table, Weibull plot with bounds, reliability curve, and a one-page PDF report named by data path
- Built-in Weibayes example for each data path. For interval data, suspensions are taken at interval ends and failures at interval midpoints (an approximation; exact when there are no failures)

### Non-parametric survival analysis

- Kaplan-Meier survival curve for individual (date/cycles) data
- Actuarial (Cutler-Ederer) life-table survival estimator for grouped and grouped interval-inspection data
- Both plot as a diagnostic overlay against your parametric fit, so you can sanity-check the Weibull/Exponential/Lognormal curve against a model-free estimate

### Crow-AMSAA (reliability growth) analysis

- Individual failure records (date/cycles-based), grouped data, or grouped interval-censored data
- IEC 61164 / MIL-HDBK-189 bias-corrected point estimator, plus Duane regression
- Automatically applies the correct failure-terminated vs. time-terminated confidence-bound formulas
- Fisher Matrix and Likelihood Ratio confidence bounds across all data-entry paths
- Laplace trend test (U-statistic), so you can check whether your data actually shows a significant trend before relying on a growth/deterioration conclusion
- Parametric bootstrap bounds for Beta and for the cumulative and instantaneous (current) MTBF
- Multi-phase analysis with manual phase boundaries or automatic change-point detection, for data whose trend changes part-way through
- "What-if" forecasting for future failures, time, and cost
- Cost & Spares Optimiser: from the fitted Beta/Lambda, computes the maintenance interval that minimises cost rate and the spares stock level that satisfies a target service level (newsvendor critical-fractile rule)

### Fleet MCF – repairable units (date / cycles data)

- Compares units of the same kind — a fleet of pumps, conveyors or haul trucks — to answer: is the fleet's repair rate rising, and which units drive it?
- Nelson mean cumulative function (MCF) with robust Lawless-Nadeau confidence bounds, plus a power-law fleet trend (Beta) with a rising / falling / no-clear-trend verdict
- Ranks every unit by its repairs against what the fleet average predicts for its observation time, and flags units that are clearly high or low (Poisson test)
- Works with any CMMS layout: map your own unit column (functional location, asset tag ...), event date or meter reading, and work-order type once — the choices are remembered by header name
- Repairs are counted; preventive work is not counted and the unit stays in service. Choose where each unit's observation starts and ends
- MCF chart with the top-ranked units overlaid, an event plot of every unit, a one-page PDF report, and a built-in six-pump example

### Plant-level annual maintenance cost forecast

A separate, plant-wide module that turns multi-year CMMS cost history into a forecast of next year's (and beyond) total maintenance cost, modeling corrective and preventive cost as two independent streams summed only at the end. See [Getting Started](#getting-started) and the in-workbook "Annual Plant Read Me" tab for the full workflow.

### Guided data entry

- Wizard for choosing your data type (individual date/cycles, grouped, grouped interval-inspection, or plant cost forecast) and pasting in CMMS/maintenance data, selecting columns, and marking failures vs. suspensions
- Step 2 analysis level: record whether you are analysing a component on one machine, a component across like machines, an equipment type or a whole plant. The name and location boxes take whatever describes your analysis; the level appears in report headers and guides the choice of method
- Step 4 "Set F / S from a column": pick the column in your CMMS export that says what kind of job each row was (order type, PM / CM, activity ...), tick the values that are repairs, and every row is marked F or S in one click — with a warning if your data has no such column
- Built-in example datasets for every data-entry path, including Weibayes and Fleet MCF examples; the date / cycles examples carry a work-order type column, as a real CMMS export would
- Calendar date-picker, and validation to catch missing or malformed data before it causes problems downstream
- The analysis choice form shows the current failure and suspension counts with a verdict for each method (Weibull, Crow-AMSAA, Weibayes), warns if failures and suspensions were changed after Step 4 or if all failures fall at one age, and offers Fleet MCF on the date / cycles paths
- Main Menu closes any windows still open, so a new analysis always starts clean

### Reporting

- One-click PDF export of the Summary pages (distribution comparison table, charts)
- Advanced report builder for a full, multi-sheet PDF of an analysis path
- Every page of a path report prints as large as fits on one page, in whichever orientation suits it
- One-page PDF reports for Weibayes and Fleet MCF, named by data path; report headers show the analysis level

## How this fits alongside commercial reliability software

This toolkit covers the core single-population life-data and reliability-growth workflow that most engineers reach a paid reliability package for: 2P/3P Weibull, exponential, and lognormal fitting by rank regression or MLE, Fisher Matrix / Likelihood Ratio / Bayesian / bootstrap confidence bounds, Weibayes zero-failure analysis with demonstration planning, non-parametric Kaplan-Meier and actuarial survival overlays, NHPP power-law (Crow-AMSAA) growth analysis with the standard bias-corrected estimators, multi-phase fits and trend testing, and Fleet MCF comparison of repairable units — all built natively in Excel/VBA, with no license fee. For a single asset, a single failure mode, a single fleet-level growth trend, or a fleet of like repairable units, the numbers should line up with what a commercial statistics or reliability package would return.

It stops short of full commercial reliability suites in a few areas: accelerated life testing (stress-life models, Arrhenius/Eyring-type extrapolation), degradation analysis, multi-stress design of experiments, warranty-data streaming from a live database, system-level modeling (reliability block diagrams, fault trees), and reliability-growth-with-corrective-actions tracking. Those are the features that typically justify an enterprise reliability platform's cost, and this workbook isn't a substitute for them.

## Requirements

- Microsoft Excel, 64-bit recommended for larger datasets (32-bit Excel's ~2GB memory limit can be a problem on bigger analyses)
- Macros must be enabled — see [Getting Started](#getting-started) below
- Windows is recommended; some features (e.g. Goal Seek-based calculations) are more reliable on Windows Excel than Excel for Mac

## Getting Started

1. Download the latest release from the [Releases page](https://weibull-nerd-eng.github.io/reliability-toolkit/).
2. Open the file in Excel. You'll see a security warning ("Protected View" or a macro warning) — this is expected for any macro-enabled workbook downloaded from the internet. Click Enable Editing, then Enable Content (or Enable Macros) to use the tool.
3. Start from the main menu and pick your data type: individual (date/cycles), grouped, grouped interval-inspection, or plant maintenance cost forecast. If it's your first time, load one of the built-in example datasets (including Weibayes and Fleet MCF examples) to see how the tool works before pasting in your own data.
4. Follow the wizard: paste in your data, select the relevant columns, and mark failures vs. suspensions — at Step 4 you can set them all at once from your work-order type column (or Preventive/Failure/Suspension for the plant cost forecast).
5. After the wizard, the analysis choice form shows whether your data suits Weibull, Crow-AMSAA or Weibayes, and on the date / cycles paths offers Fleet MCF to compare repairable units. For Weibayes, set units still running to S first, then click Weibayes – zero / few failures.
6. For the plant cost forecast specifically, see the "Annual Plant Read Me" tab in the workbook for the full workflow, required Settings-form inputs, and its limitations.
7. See [docs/screenshots/](docs/screenshots/) for a walkthrough of the main screens.

If Excel won't let you enable macros at all, your organization's security policy may be blocking it — check with IT, or run the file on a personal machine.

## Disclaimer

See [DISCLAIMER.md](https://github.com/Weibull-Nerd-eng/reliability-toolkit/blob/main/DISCLAIMER.md). Short version: this is free, provided as-is, with no warranty — verify results independently before using them for real maintenance or safety decisions. The plant cost forecast includes an approximate uncertainty band, Weibayes results depend on the Beta you assume, and Fleet MCF flags are screening indicators — treat all three as planning-level figures.

## Support this project

If this tool has been useful to you, tips are welcome and appreciated (never required): [ko-fi.com/weibullnerdeng](https://ko-fi.com/weibullnerdeng)

## Reporting issues / requesting features

Found a bug or have a suggestion? Please [open an Issue](https://github.com/Weibull-Nerd-eng/reliability-toolkit/issues) — include what you were doing, what you expected, and (if possible) a screenshot or the specific values involved. That's the single most useful thing you can include for a fast fix.

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for the list of changes in each version.

## License

All rights reserved. This workbook is free to download and use for personal and internal business purposes. Redistribution, resale, or modification without permission is not permitted. See [DISCLAIMER.md](https://github.com/Weibull-Nerd-eng/reliability-toolkit/blob/main/DISCLAIMER.md) for the full usage terms.
