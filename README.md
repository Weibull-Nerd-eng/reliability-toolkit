[README.md](https://github.com/user-attachments/files/30489606/README.md)
# Bi/Tri Weibull & Crow-AMSAA Reliability Analysis Toolkit

A free, macro-enabled Excel workbook for reliability engineering analysis — Weibull life data analysis and Crow-AMSAA (NHPP power-law) reliability growth analysis, with full confidence-bound calculations, built entirely in native Excel + VBA. No add-ins, no external software required.

## What it does

**Weibull analysis**
- 2-parameter and 3-parameter (Tri) Weibull fitting
- Rank regression and Maximum Likelihood Estimation (MLE)
- Supports suspended/censored data, grouped data, and grouped interval-inspection data
- Confidence bounds: Fisher Matrix, Likelihood Ratio, and Bayesian credible bounds (with your own prior)
- Spares forecasting, cost analysis, and goodness-of-fit reporting

**Crow-AMSAA (reliability growth) analysis**
- Individual failure records (date/cycles-based) or interval-censored grouped data
- IEC 61164 / MIL-HDBK-189 bias-corrected point estimator, plus Duane regression
- Fisher Matrix and Likelihood Ratio confidence bounds across all data-entry paths
- "What-if" forecasting for future failures, time, and cost

**Guided data entry**
- Step-by-step wizard for pasting in CMMS/maintenance data, selecting the relevant columns, and marking failures vs. suspensions
- Built-in validation to catch missing data before it causes problems downstream

## Requirements

- Microsoft Excel, **64-bit recommended** for larger datasets (32-bit Excel's ~2GB memory limit can be a problem on bigger analyses)
- Macros must be enabled — see [Getting Started](#getting-started) below
- Windows is recommended; some features (e.g. Goal Seek-based calculations) are more reliable on Windows Excel than Excel for Mac

## Getting Started

1. Download the latest release from the [Releases](../../releases) page.
2. Open the file in Excel. You'll see a security warning ("Protected View" or a macro warning) — **this is expected** for any macro-enabled workbook downloaded from the internet. Click **Enable Editing**, then **Enable Content** (or **Enable Macros**) to use the tool.
3. Start from the main menu / "Start Analysis" button and follow the wizard: paste in your data, select the relevant columns, and mark failures vs. suspensions.
4. See `docs/screenshots/` for a walkthrough of the main screens.

> If Excel won't let you enable macros at all, your organization's security policy may be blocking it — check with IT, or run the file on a personal machine.

## Disclaimer

See [DISCLAIMER.md](DISCLAIMER.md). Short version: this is free, provided as-is, with no warranty — verify results independently before using them for real maintenance or safety decisions.

## Support this project

If this tool has been useful to you, tips are welcome and appreciated (never required): **[Ko-fi link here]**

## Reporting issues / requesting features

Found a bug or have a suggestion? Please open an [Issue](../../issues) — include what you were doing, what you expected, and (if possible) a screenshot or the specific values involved. That's the single most useful thing you can include for a fast fix.

## Changelog

See [CHANGELOG.md](CHANGELOG.md).

## License

All rights reserved. This workbook is free to download and use for personal and internal business purposes. Redistribution, resale, or modification without permission is not permitted. See [DISCLAIMER.md](DISCLAIMER.md) for the full usage terms.
