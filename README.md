# Bi/Tri Weibull & Crow-AMSAA Reliability Analysis Toolkit

**Current version: v3.0** · [Download the latest release](https://github.com/Weibull-Nerd-eng/reliability-toolkit/releases/latest) · [What's new](CHANGELOG.md)

A free, macro-enabled Excel workbook for reliability engineering analysis — Weibull/Exponential/Lognormal life data analysis, Crow-AMSAA (NHPP power-law) reliability growth analysis, non-parametric survival analysis, and plant-level maintenance cost forecasting — with full confidence-bound calculations, built entirely in native Excel + VBA. No add-ins, no external software required.

For a full feature breakdown see [FEATURES.md](FEATURES.md). For example screens from a real analysis see [SAMPLE-OUTPUT.md](SAMPLE-OUTPUT.md).

## What's new in v3.0

- **Bootstrap confidence intervals** for Bi/Tri Weibull (individual, grouped and grouped interval-inspection data) and for Crow-AMSAA on every data-entry path.
- **Multi-phase Crow-AMSAA** with a separate Beta/Lambda for each phase, phase charts, and change-point detection with a suggested phase boundary.
- **Duane reliability growth model** and **cumulative / instantaneous MTBF with confidence bounds** on every Crow-AMSAA path.
- **Beta-binomial confidence band** on the Weibull regression chart, replacing straight-line trendlines.
- **Improved cost and spares analysis**: optional replacement downtimes, exact cost optimum, cost Pareto and uncertainty band, and a selectable spares confidence level.
- **Rewritten Grouped Weibull MLE** (interval-censored likelihood weighted by recorded counts) and a **Solver-free** Tri-model shift fit.
- **Safer workflow**: input and interval checks, prompts before overwriting results, a non-destructive Back button, and a 15,000-row data limit with a warning.

Results for Grouped Weibull fits, Tri-model shift parameters and Grouped confidence bands can differ from REV 2.0, so re-run existing analyses rather than relying on saved REV 2.0 output. The full list, including fixes and removals, is in [CHANGELOG.md](CHANGELOG.md).

## What it does

### Life data analysis (Weibull / Exponential / Lognormal)

- 2-parameter and 3-parameter ("Tri") Weibull fitting, plus Exponential and Lognormal fits for side-by-side distribution comparison — all available across every data-entry path (individual records, grouped data, and grouped interval-inspection data)
- Rank regression and Maximum Likelihood Estimation (MLE)
- Supports suspended/censored data, grouped data, and grouped interval-inspection data
- Confidence bounds: Fisher Matrix, Likelihood Ratio, Bayesian credible bounds (with your own prior), and parametric bootstrap intervals
- Beta-binomial confidence band on the Weibull regression chart
- Goodness-of-fit reporting (Anderson-Darling AD*, Stephens-adjusted) and a Distribution Fit Comparison table to help you pick the best-fitting model
- Spares forecasting and cost analysis built on the fitted distribution, with optional replacement downtimes and a selectable spares confidence level

### Non-parametric survival analysis

- Kaplan-Meier survival curve for individual (date/cycles) data
- Actuarial (Cutler-Ederer) life-table survival estimator for grouped and grouped interval-inspection data
- Both plot as a diagnostic overlay against your parametric fit, so you can sanity-check the Weibull/Exponential/Lognormal curve against a model-free estimate, with a "Reading the chart" table alongside

### Crow-AMSAA (reliability growth) analysis

- Individual failure records (date/cycles-based), grouped data, or grouped interval-censored data
- IEC 61164 / MIL-HDBK-189 bias-corrected point estimator, plus Duane regression
- Automatically applies the correct failure-terminated vs. time-terminated confidence-bound formulas
- Fisher Matrix and Likelihood Ratio confidence bounds across all data-entry paths, plus bootstrap intervals
- Cumulative and instantaneous MTBF with confidence bounds across all data-entry paths
- Multi-phase analysis with change-point detection for data whose growth rate changes over time
- Laplace trend test (U-statistic), so you can check whether your data actually shows a significant trend before relying on a growth/deterioration conclusion
- "What-if" forecasting for future failures, time, and cost
- Cost & Spares Optimiser: from the fitted Beta/Lambda, computes the maintenance interval that minimises cost rate and the spares stock level that satisfies a target service level (newsvendor critical-fractile rule)

### Plant-level annual maintenance cost forecast

A separate, plant-wide module that turns multi-year CMMS cost history into a forecast of next year's (and beyond) total maintenance cost, modeling corrective and preventive cost as two independent streams summed only at the end. The results are on the **Maintenance Cost Forecast** sheet (called "Output" in REV 2.0). See [Getting Started](#getting-started) and the in-workbook "Annual Plant Read Me" tab for the full workflow.

### Guided data entry

- Wizard for choosing your data type (individual date/cycles, grouped, grouped interval-inspection, or plant cost forecast) and pasting in CMMS/maintenance data, selecting columns, and marking failures vs. suspensions
- Built-in example datasets for every data-entry path
- Calendar date-picker, and validation to catch missing or malformed data (including interval order and count checks) before it causes problems downstream

### Reporting

- One-click PDF export of the Summary pages (distribution comparison table, charts)
- Advanced report builder for a full, multi-sheet PDF of an analysis path

## How this fits alongside commercial reliability software

This toolkit covers the core single-population life-data and reliability-growth workflow that most engineers reach a paid reliability package for: 2P/3P Weibull, exponential, and lognormal fitting by rank regression or MLE, Fisher Matrix / Likelihood Ratio / Bayesian / bootstrap confidence bounds, non-parametric Kaplan-Meier and actuarial survival overlays, and NHPP power-law (Crow-AMSAA) growth analysis, including multi-phase analysis, with the standard bias-corrected estimators and trend testing — all built natively in Excel/VBA, with no license fee. For a single asset, a single failure mode, or a single fleet-level growth trend, the numbers should line up with what a commercial statistics or reliability package would return.

It stops short of full commercial reliability suites in a few areas: accelerated life testing (stress-life models, Arrhenius/Eyring-type extrapolation), degradation analysis, multi-stress design of experiments, warranty-data streaming from a live database, system-level modeling (reliability block diagrams, fault trees), and reliability-growth-with-corrective-actions tracking. Those are the features that typically justify an enterprise reliability platform's cost, and this workbook isn't a substitute for them.

## Requirements

- Microsoft Excel, 64-bit recommended for larger datasets (32-bit Excel's ~2GB memory limit can be a problem on bigger analyses)
- Macros must be enabled — see [Getting Started](#getting-started) below
- Windows is recommended; some features (e.g. Goal Seek-based calculations) are more reliable on Windows Excel than Excel for Mac
- Input sheets read data down to row 15,000 (14,993 records); the workbook warns you if data sits below that limit

## Getting Started

1. Download the latest release from the [Releases page](https://github.com/Weibull-Nerd-eng/reliability-toolkit/releases/latest).
2. Open the file in Excel. You'll see a security warning ("Protected View" or a macro warning) — this is expected for any macro-enabled workbook downloaded from the internet. Click **Enable Editing**, then **Enable Content** (or **Enable Macros**) to use the tool.
3. Start from the main menu and pick your data type: individual (date/cycles), grouped, grouped interval-inspection, or plant maintenance cost forecast. If it's your first time, load one of the built-in example datasets to see how the tool works before pasting in your own data.
4. Follow the wizard: paste in your data, select the relevant columns, and mark failures vs. suspensions (or Preventive/Failure/Suspension for the plant cost forecast).
5. For the plant cost forecast specifically, see the "Annual Plant Read Me" tab in the workbook for the full workflow, required Settings-form inputs, and its limitations.
6. See [docs/screenshots/](docs/screenshots/) for a walkthrough of the main screens, and [SAMPLE-OUTPUT.md](SAMPLE-OUTPUT.md) for example output.

If Excel won't let you enable macros at all, your organization's security policy may be blocking it — check with IT, or run the file on a personal machine.

### Upgrading from REV 2.0

Download v3.0 from the Releases page and use it for new analyses. The earlier REV 2.0 workbook is still available from the [Ver.2 release](https://github.com/Weibull-Nerd-eng/reliability-toolkit/releases/tag/Ver.2). Results are not carried across files, so re-run existing analyses in v3.0. See the "Upgrade notes" in [CHANGELOG.md](CHANGELOG.md).

## Disclaimer

See [DISCLAIMER.md](DISCLAIMER.md). Short version: this is free, provided as-is, with no warranty — verify results independently before using them for real maintenance or safety decisions. The plant cost forecast in particular is a planning estimate with only an approximate uncertainty band — treat it as an order-of-magnitude planning number.

## Support this project

If this tool has been useful to you, tips are welcome and appreciated (never required): <https://ko-fi.com/weibullnerdeng>

## Reporting issues / requesting features

Found a bug or have a suggestion? Please [open an Issue](https://github.com/Weibull-Nerd-eng/reliability-toolkit/issues) — include what you were doing, what you expected, and (if possible) a screenshot or the specific values involved. That's the single most useful thing you can include for a fast fix.

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for the list of changes in each version.

## License

All rights reserved. This workbook is free to download and use for personal and internal business purposes. Redistribution, resale, or modification without permission is not permitted. See [DISCLAIMER.md](DISCLAIMER.md) for the full usage terms.
