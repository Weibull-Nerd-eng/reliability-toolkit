# Feature Summary

## Weibull Life Data Analysis (2-Parameter & 3-Parameter)

Every path below supports both a standard **2-parameter Weibull** fit and a **3-parameter ("Tri") Weibull** fit with a fitted location/threshold parameter, across three major data structures:

### 1. Base — individual failure & suspension records
Paste in individual failure/suspension records directly. Supports:
- Rank regression and Maximum Likelihood Estimation (MLE) fitting
- Fisher Matrix, Likelihood Ratio, and Bayesian credible confidence bounds (90% two-sided)
- Bayesian MAP (maximum a posteriori) Beta estimation with a user-supplied prior (expected Beta and prior sample size), for incorporating engineering judgment or fleet history alongside the data
- Goodness-of-fit reporting

### 2. Grouped — binned/grouped failure data
Same fitting methods and confidence bound options as Base, adapted for data already grouped into bins/intervals rather than individual timestamps.

### 3. Grouped Inspection — interval-censored inspection data
For data from periodic inspections where failures are only known to have occurred sometime between two inspection dates, rather than at a precise time. Same fitting and bounding methods, built for this larger, interval-censored data structure.

**Across all three paths:** guided data-entry wizard, rank-order/probability plotting worksheets, and a Weibull shape parameter explanation reference sheet.

---

## Crow-AMSAA Reliability Growth Analysis (NHPP Power-Law Process)

Same three-major-path structure as Weibull, for tracking whether a system's reliability is improving, degrading, or stable over time/usage:

### 1. Individual records — date-based or cycles-based
Paste in individual failure records against either calendar dates or a usage/cycles count. Point estimate via the classical IEC 61164 / MIL-HDBK-189 bias-corrected estimator, plus an independent Duane log-log regression estimate. Closed-form chi-square confidence bounds (Beta, cumulative MTBF, and implied Lambda). Each has a companion sheet for the other test-termination convention (failure-terminated vs. time-terminated — see note above).

### 2. Grouped — interval failure counts
For data recorded as failure counts per time interval rather than individual timestamps. Point estimate via Goal Seek MLE on the grouped-data score equation. Fisher Matrix and Likelihood Ratio confidence bounds.

### 3. Grouped Inspection — larger interval-censored datasets
Same Goal Seek MLE and Fisher Matrix/Likelihood Ratio bounds as Grouped, sized for much larger inspection-based datasets.

**Across all paths:** "What-if" forecasting — enter a future time/cycles or failure count and see the expected corresponding value, with results linked directly to the reliability growth plots.

---

## Cost Analysis

A dedicated cost comparison sheet for every one of the six Weibull paths (Base, Tri, Grouped, GroupedTri, GroupedInspection, GroupedInspectionTri) — compares costs based on the fitted life distribution, to support run-to-failure vs. planned-replacement cost tradeoff decisions.

## Spares Forecasting

A matching spare-parts forecasting sheet for all six Weibull paths, projecting expected spares demand based on the fitted failure distribution.

## Confidence Bounds — full picture

Across both Weibull and Crow-AMSAA, three independent bounding methods are available (90% two-sided):

- **Fisher Matrix bounds** — the standard asymptotic approach, fast to compute directly from the likelihood.
- **Likelihood Ratio bounds** — generally more accurate than Fisher Matrix for smaller sample sizes, computed via custom golden-section-search/bisection root-finding.
- **Bayesian credible bounds** — incorporates a user-supplied prior belief about the shape parameter (with adjustable weighting), available across all six Weibull paths via the on-sheet Bayesian button.

Crow-AMSAA's individual-record paths (date/Cycles) use the classical closed-form chi-square "Crow bounds" instead of Fisher/Likelihood Ratio, consistent with standard reliability growth analysis practice.

---

## Supporting features

- Guided CMMS data-entry wizard — paste in raw maintenance data, select the relevant columns, and mark failures vs. suspensions, with validation for missing data
- Example dataset included for first-time use
- Goodness-of-fit reporting
- Built-in help topics, including an explanation of failure-terminated vs. time-terminated test data
- Automated error handling with descriptive messages throughout
