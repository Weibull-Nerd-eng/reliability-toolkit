# Feature Summary

## Life Data Analysis: Weibull (2P & 3P), Exponential, Lognormal

Every path below supports a standard **2-parameter Weibull** fit, a **3-parameter ("Tri") Weibull** fit with a fitted location/threshold parameter, an **Exponential** fit, and a **Lognormal** fit — all fitted from the same dataset for direct side-by-side comparison — across three major data structures:

### 1. Base — individual failure & suspension records

Paste in individual failure/suspension records directly. Supports:

- Rank regression and Maximum Likelihood Estimation (MLE) fitting
- Fisher Matrix, Likelihood Ratio, and Bayesian credible confidence bounds (90% two-sided)
- Bayesian MAP (maximum a posteriori) Beta estimation with a user-supplied prior (expected Beta and prior sample size), for incorporating engineering judgment or fleet history alongside the data
- Goodness-of-fit reporting (Anderson-Darling AD*, Stephens-adjusted) and a Distribution Fit Comparison table across Weibull/Exponential/Lognormal
- Kaplan-Meier non-parametric survival curve, plotted as an overlay for a model-free sanity check against the parametric fit

### 2. Grouped — binned/grouped failure data

Same fitting methods, confidence bound options, and distribution comparison as Base, adapted for data already grouped into bins/intervals rather than individual timestamps. Includes an Actuarial (Cutler-Ederer) life-table survival estimator as the non-parametric overlay, built for interval-summarized counts rather than individual event times.

### 3. Grouped Inspection — interval-censored inspection data

For data from periodic inspections where failures are only known to have occurred sometime between two inspection dates, rather than at a precise time. Same fitting, bounding, and Actuarial survival overlay as Grouped, built for this larger, interval-censored data structure.

**Across all three paths:** guided data-entry wizard, rank-order/probability plotting worksheets, and a Weibull shape parameter explanation reference sheet.

---

## Crow-AMSAA Reliability Growth Analysis (NHPP Power-Law Process)

Same three-major-path structure as the life-data analysis, for tracking whether a system's reliability is improving, degrading, or stable over time/usage:

### 1. Individual records — date-based or cycles-based

Paste in individual failure records against either calendar dates or a usage/cycles count. Point estimate via the classical IEC 61164 / MIL-HDBK-189 bias-corrected estimator, plus an independent Duane log-log regression estimate. Closed-form chi-square confidence bounds (Beta, cumulative MTBF, and implied Lambda). Each has a companion sheet for the other test-termination convention (failure-terminated vs. time-terminated). A Laplace trend test (U-statistic) reports whether the data shows a statistically significant trend at all, before you draw a growth/deterioration conclusion from the point estimate.

### 2. Grouped — interval failure counts

For data recorded as failure counts per time interval rather than individual timestamps. Point estimate via Goal Seek MLE on the grouped-data score equation. Fisher Matrix and Likelihood Ratio confidence bounds.

### 3. Grouped Inspection — larger interval-censored datasets

Same Goal Seek MLE and Fisher Matrix/Likelihood Ratio bounds as Grouped, sized for much larger inspection-based datasets.

**Across all paths:** "What-if" forecasting — enter a future time/cycles or failure count and see the expected corresponding value, with results linked directly to the reliability growth plots.

---

## Cost & Spares Optimizer

Using the Beta/Lambda fitted from the Crow-AMSAA analysis, computes the maintenance interval that minimizes cost rate and the spares stock level that satisfies a target service level (newsvendor critical-fractile rule). Available for date, cycles, grouped, and grouped interval-inspection data.

## Cost Analysis

A dedicated cost comparison sheet for every one of the Weibull paths (Base, Tri, Grouped, GroupedTri, GroupedInspection, GroupedInspectionTri) — compares costs based on the fitted life distribution, to support run-to-failure vs. planned-replacement cost tradeoff decisions.

## Spares Forecasting

A matching spare-parts forecasting sheet for the Weibull paths above, projecting expected spares demand based on the fitted failure distribution.

## Confidence Bounds — full picture

Across Weibull, Exponential, Lognormal, and Crow-AMSAA, three independent bounding methods are available (90% two-sided):

- **Fisher Matrix bounds** — the standard asymptotic approach, fast to compute directly from the likelihood.
- **Likelihood Ratio bounds** — generally more accurate than Fisher Matrix for smaller sample sizes, computed via custom golden-section-search/bisection root-finding.
- **Bayesian credible bounds** — incorporates a user-supplied prior belief about the shape parameter (with adjustable weighting), available across all six Weibull paths via the on-sheet Bayesian button.

Crow-AMSAA's individual-record paths (date/cycles) use the classical closed-form chi-square "Crow bounds" instead of Fisher/Likelihood Ratio, consistent with standard reliability growth analysis practice.

---

## Plant-Level Annual Maintenance Cost Forecast

A separate, plant-wide module (distinct from the per-asset Cost Analysis sheets above) that turns a multi-year CMMS cost history into a forecast of next year's — and beyond — total maintenance cost.

**Structure:** corrective (failure) cost and preventive (PM) cost are modeled as two independent streams, only summed at the end:

- **Corrective cost** — a Crow-AMSAA fit on annual failure counts, times a separately modeled cost-per-failure. Fitting a single curve directly through cumulative cost is deliberately avoided, since that lets cost-per-event noise distort the fitted growth exponent, with no basis in the Crow-AMSAA / MIL-HDBK-189 literature.
- **Preventive cost** — its own flat, inflation-adjusted average, since scheduled maintenance isn't a stochastic failure process and Crow-AMSAA shouldn't be used to forecast it.

**Basis:** the corrective-cost structure mirrors a published case study — Comerford, Areva T&D NZ (VANZ, 2005) — which applied Crow-AMSAA to Meridian Energy's fleet of 38 dissimilar hydro units, pooled as one plant-level forced-outage trend, multiplied by an independently built cost-per-outage figure. This pooled result speaks to the plant's overall corrective burden, not to which specific asset fails next; for asset-level resolution, filter the data input by Equipment Class and run the module separately per class.

**Workflow:** import or paste a CMMS export (any columns, any order) → filter out rows you don't want included → map columns (Equipment ID, Event Date, Cost; Work Order and Equipment Class optional) and classify each row as Failure/Preventive/Suspension → refresh the forecast → read the Output report. A Quick Start guide and worked example dataset are built in.

**Configurable settings:** fiscal year start month, years of history to include, fallback flat inflation rate (with per-year override), years to forecast, and whether corrective cost-per-failure uses a flat historical average or a fitted trend.

**Limitations (documented on the "Annual Plant Read Me" sheet):**

- This is a point forecast, not a confidence interval — treat it as an order-of-magnitude planning number with only a handful of years of annual data.
- A single Beta assumes the plant's underlying failure process is stable over the history used; a major turnaround, capital project, or equipment-mix shift will show as a kink in the fit, and should be handled by restricting years of history.
- Preventive cost is a flat historical average by design and won't capture a materially growing or shrinking PM program.

---

## Non-Parametric Survival Analysis

- **Kaplan-Meier** survival curve for individual (date/cycles) data.
- **Actuarial (Cutler-Ederer) life-table** survival estimator for grouped and grouped interval-inspection data, correctly handling interval-summarized counts rather than individual event times.
- Both are shown as a diagnostic overlay against the parametric fit, so the Weibull/Exponential/Lognormal curve can be sanity-checked against a model-free estimate.

## Reporting

- One-click PDF export of the Summary pages (distribution comparison table and charts).
- Advanced report builder for a full, multi-sheet PDF of a given analysis path.

## Supporting features

- Guided data-entry wizard — choose your data type (individual, grouped, grouped interval-inspection, or plant cost forecast), paste in raw maintenance data, select the relevant columns, and mark failures vs. suspensions, with validation for missing data
- Example dataset included for every data-entry path
- Calendar date-picker for entering dates
- Goodness-of-fit reporting
- Built-in help topics, including an explanation of failure-terminated vs. time-terminated test data
- Automated error handling with descriptive messages throughout

---

## Relationship to commercial reliability software (brief)

The life-data (Weibull/Exponential/Lognormal), non-parametric survival, and Crow-AMSAA modules above cover the same statistical ground as the single-population analysis tools in dedicated commercial reliability packages — same estimators (rank regression, MLE, IEC 61164/MIL-HDBK-189), same bounding methods (Fisher Matrix, Likelihood Ratio, plus a Bayesian option many packages charge extra for), and the same goodness-of-fit diagnostics. What isn't here, and what typically justifies a commercial platform's license cost instead: accelerated life testing and stress-life extrapolation, degradation modeling, system-level reliability block diagrams or fault trees, and formal reliability-growth-with-corrective-actions tracking. If your analysis is single-asset or single-fleet life data or growth trending, this workbook should get you to the same numbers; for accelerated testing or system-level modeling, use a dedicated platform.

