# Feature Summary

*Applies to v3.1. See [CHANGELOG.md](CHANGELOG.md) for what changed in each version, and the [README](README.md) for an overview and download link.*

## Life Data Analysis: Weibull (2P & 3P), Exponential, Lognormal

Every path below supports a standard **2-parameter Weibull** fit, a **3-parameter ("Tri") Weibull** fit with a fitted location/threshold parameter, an **Exponential** fit, and a **Lognormal** fit — all fitted from the same dataset for direct side-by-side comparison — across three major data structures. The Tri-model location parameter is fitted with a built-in routine, so no Excel Solver is needed.

### 1. Base — individual failure & suspension records

Paste in individual failure/suspension records directly, by calendar date or by cycles/operating hours. Supports:

- Rank regression and Maximum Likelihood Estimation (MLE) fitting, with a reduced-bias (RBA) MLE Beta for small samples
- Fisher Matrix, Likelihood Ratio, and Bayesian credible confidence bounds (90% two-sided), plus parametric bootstrap intervals
- 90% beta-binomial confidence band drawn on the Weibull regression chart
- Bayesian MAP (maximum a posteriori) Beta estimation with a user-supplied prior (expected Beta and prior sample size), for incorporating engineering judgment or fleet history alongside the data
- Goodness-of-fit reporting (Anderson-Darling AD*, Stephens-adjusted) and a Distribution Fit Comparison table across Weibull/Exponential/Lognormal
- Kaplan-Meier non-parametric survival curve, plotted as an overlay for a model-free sanity check against the parametric fit
- Weibull future-failure forecast for the units still running

### 2. Grouped — binned/grouped failure data

Same fitting methods, confidence bound options, and distribution comparison as Base, adapted for data already grouped into bins/intervals rather than individual timestamps. The Weibull MLE uses an interval-censored likelihood weighted by the recorded failure and suspension counts, and the confidence band uses the correct sample size and ranks for grouped data. Goodness of fit is reported as G² deviance with a p-value and AIC. Includes an Actuarial (Cutler-Ederer) life-table survival estimator as the non-parametric overlay, built for interval-summarized counts rather than individual event times.

### 3. Grouped Inspection — interval-censored inspection data

For data from periodic inspections where failures are only known to have occurred sometime between two inspection dates, rather than at a precise time. Same fitting, bounding, G² / AIC fit checks and Actuarial survival overlay as Grouped, built for this larger, interval-censored data structure. Interval order and count checks run before the analysis starts.

**Across all three paths:** guided data-entry wizard, rank-order/probability plotting worksheets, and bootstrap results sheets. The REV 2.0 Weibull shape explanation sheet has been removed.

---

## Weibayes — Zero-Failure / Few-Failure Analysis

For when you have no failures, or only one or two, and want to know what life you can claim. Available on all three data paths (Base date/cycles, Grouped, and Grouped Inspection) from the analysis choice form.

- Uses the running time of every unit and an assumed or historical **Beta** (defaults to the last Weibull fit on that path), with the Failure/Suspension flags exactly as they are on the input sheet
- One-sided **lower bound on Eta** and on the **B-life** (e.g. B10), **reliability at a mission time**, plus an upper bound and the Weibayes estimate when there are failures
- **Beta sensitivity table** across the credible Beta range, so you can quote the least favourable case
- **Demonstration planning** — whether a target B-life is already demonstrated, how much more failure-free running each unit needs, or how long new test units must run; a **What-If form** suggests realistic targets from your own data and previews the running time each one needs
- Weibull plot (Weibayes line with lower/upper bounds, failures and running units) and reliability curve marked with mission time, oldest unit and target B-life
- Interval data handled by the standard convention: suspensions at interval ends, failures at interval midpoints (an approximation; exact when there are no failures). The plot shows one point per inspection, at the interval end
- One-page PDF report, named by data path; a worked example for each data path

---

## Reliability Growth: Crow-AMSAA

Same three-major-path structure as the life-data analysis, for tracking whether a system's failure rate is improving (reliability growth) or worsening (deterioration) over time. Each path can be run on failures only, or on all maintenance events.

### 1. Individual records — date-based or cycles-based

Paste in individual failure records against either calendar dates or a usage/cycles count. Point estimate via the classical IEC 61164 / MIL-HDBK-189 bias-corrected estimator, plus an independent Duane log-log regression estimate with its own chart. Closed-form chi-square confidence bounds (Beta, cumulative MTBF, and implied Lambda). Each has a companion sheet for the other test-termination convention (failure-terminated vs. time-terminated). A Laplace trend test (U-statistic) reports whether the data shows a statistically significant trend at all, before you draw a growth/deterioration conclusion from the point estimate.

### 2. Grouped — interval failure counts

For data recorded as failure counts per time interval rather than individual timestamps. Maximum likelihood estimates are solved with Goal Seek, with Fisher Matrix and Likelihood Ratio confidence bounds and a Laplace trend test using interval midpoints.

### 3. Grouped Inspection — interval-censored inspection data

Same Goal Seek MLE and Fisher Matrix/Likelihood Ratio bounds as Grouped, sized for much larger inspection-based datasets.

**Across all paths:**

- **"What-if" forecasting** — enter a future time/cycles or failure count and see the expected corresponding value, with results linked directly to the reliability growth plots. A forecast you have entered is kept when you leave and return.
- **Cumulative and instantaneous MTBF** with Fisher Matrix and Likelihood Ratio bounds.
- **Bootstrap confidence intervals** — parametric bootstrap of the power-law process, with the right failure- or time-terminated behaviour.
- **Multi-phase analysis** — fit a separate Beta and Lambda to each phase, with phase charts, for data whose growth rate changes over time. A change-point detector suggests a best phase boundary, and you can enter your own.
- **Duane model** built onto the same sheets as the Crow-AMSAA fit.
- **Fleet MCF** — on the date/cycles paths, a companion analysis that keeps units separate rather than pooling them into one growth curve (see *Fleet MCF* below).

---

## Fleet MCF — Repairable Units (Date / Cycles)

Compares repairable units of the same kind — a fleet of pumps, conveyors or haul trucks — to answer two questions Crow-AMSAA's pooled curve can't: *is the fleet's repair rate rising, and which units drive it?* Available on the individual date and cycles paths (it needs a unit identifier on every work order), from the analysis choice form and the Crow-AMSAA menu.

- **Nelson mean cumulative function (MCF)** — average repairs per unit by age — with robust **Lawless-Nadeau** confidence bounds that stay valid when units behave differently
- **Fleet trend** — power-law (multiple-system Crow) Beta with bounds and a plain verdict: rising (wear-out), falling (improving), or no clear trend
- **Units ranked** — each unit's repairs against what the fleet average predicts for its own observation time, with **HIGH / low** flags from a Poisson test
- **Any CMMS layout** — map your own unit column (functional location, asset tag, equipment number …), event date or meter reading, and work-order type, and tick which values mean a repair. Choices are remembered by header name, so a re-ordered export is still recognised; unit names are matched with spaces trimmed and case ignored
- Repairs are counted; preventive work is not counted and the unit stays in service. Each unit's observation window can start at the earliest date, its own first record, or a fixed date, and end at the analysis date, its last record, or a fixed date (cycles: the meter reading is the unit's age)
- MCF chart with bounds, the power-law curve and the top-ranked units overlaid; an event plot of every unit's window, repairs and PMs
- One-page PDF report, named by data path; a six-pump worked example with typical CMMS headers

---

## Cost & Spares Optimiser

Using the Beta/Lambda fitted from the Crow-AMSAA analysis, computes the maintenance interval that minimises cost rate and the spares stock level that satisfies a target service level (newsvendor critical-fractile rule). Available for date, cycles, grouped, and grouped interval-inspection data. Inputs are set on the optimisation forms.

## Cost Analysis

A dedicated cost comparison sheet for every one of the Weibull paths (Base, Tri, Grouped, GroupedTri, GroupedInspection, GroupedInspectionTri) — compares costs based on the fitted life distribution, to support run-to-failure vs. planned-replacement cost tradeoff decisions.

- Optional replacement downtimes: the time a planned replacement takes and the time to repair after a failure, which lengthen each replacement cycle. Leave them blank for the classic age-replacement model.
- The cost-optimal replacement age is found exactly, not only on a whole-unit age grid.

## Spares Forecasting

A matching spare-parts forecasting sheet for the Weibull paths above, projecting expected spares demand based on the fitted failure distribution. You choose the confidence level used for the recommended stock level, and the result is shown on the page.

## Confidence Bounds — full picture

Across Weibull, Exponential, Lognormal, and Crow-AMSAA, independent bounding methods are available (90% two-sided):

- **Fisher Matrix bounds** — the standard asymptotic approach, fast to compute directly from the likelihood.
- **Likelihood Ratio bounds** — generally more accurate than Fisher Matrix for smaller sample sizes, computed via custom golden-section-search/bisection root-finding.
- **Bayesian credible bounds** — incorporates a user-supplied prior belief about the shape parameter (with adjustable weighting), available across all six Weibull paths via the on-sheet Bayesian button.
- **Parametric bootstrap intervals** — available for Bi- and Tri-Weibull on all three data paths and for Crow-AMSAA on all of its paths, with a results sheet for each and a summary on the Summary page.

Crow-AMSAA's individual-record paths (date/cycles) use the classical closed-form chi-square "Crow bounds" instead of Fisher/Likelihood Ratio, consistent with standard reliability growth analysis practice.

Weibayes uses chi-square bounds on Eta for an assumed Beta (one-sided, at the confidence you choose), and Fleet MCF uses Lawless-Nadeau robust bounds on the mean cumulative function, with chi-square bounds on the fleet Beta.

---

## Plant-Level Annual Maintenance Cost Forecast

A separate, plant-wide module (distinct from the per-asset Cost Analysis sheets above) that turns multi-year CMMS cost history into a forecast of next year's (and later years') total maintenance cost. Corrective and preventive cost are modelled as two independent streams and summed only at the end: corrective cost from a Crow-AMSAA trend in failure counts multiplied by a cost per failure, preventive cost from the historical average.

**Basis:** the corrective-cost structure mirrors a published case study — Comerford, Areva T&D NZ (VANZ, 2005) — which applied Crow-AMSAA to Meridian Energy's fleet of 38 dissimilar hydro units, pooled as one plant-level forced-outage trend, multiplied by an independently built cost-per-outage figure. This pooled result speaks to the plant's overall corrective burden, not to which specific asset fails next; for asset-level resolution, filter the data input by Equipment Class and run the module separately per class.

**Workflow:** import or paste a CMMS export (any columns, any order) → filter out rows you don't want included → map columns (Equipment ID, Event Date, Cost; Work Order and Equipment Class optional) and classify each row as Failure/Preventive/Suspension → refresh the forecast → read the **Maintenance Cost Forecast** sheet (called "Output" in REV 2.0). A Quick Start guide and worked example dataset are built in. The report also includes a cost Pareto and an approximate 90% interval on the projected total.

**Configurable settings:** fiscal year start month, years of history to include, fallback flat inflation rate (with per-year override), years to forecast, and whether corrective cost-per-failure uses a flat historical average or a fitted trend.

**Limitations (documented on the "Annual Plant Read Me" sheet):**

- The forecast is a planning estimate with only an approximate uncertainty band — treat it as an order-of-magnitude planning number with only a handful of years of annual data.
- A single Beta assumes the plant's underlying failure process is stable over the history used; a major turnaround, capital project, or equipment-mix shift will show as a kink in the fit, and should be handled by restricting years of history.
- Preventive cost is a flat historical average by design and won't capture a materially growing or shrinking PM program.
- The result is only as good as the CMMS history: misclassified work orders (a breakdown logged as preventive, or the reverse) shift cost between the two streams.

---

## Non-parametric Survival Analysis

- **Kaplan-Meier** survival curve for individual (date/cycles) data.
- **Actuarial (Cutler-Ederer) life-table** survival estimator for grouped and grouped interval-inspection data, correctly handling interval-summarized counts rather than individual event times.
- Both are shown as a diagnostic overlay against the parametric fit, so the Weibull/Exponential/Lognormal curve can be sanity-checked against a model-free estimate, with a "Reading the chart" table of survival values at evenly spaced ages.

## Reporting

- One-click PDF export of the Summary pages (distribution comparison table, charts)
- Path report builder for a full, multi-sheet PDF of everything run on an analysis path
- Path reports print every page as large as fits on one page, in whichever orientation suits it; input shading is left off the PDF and chart labels are kept clear of axes and curves
- One-page Weibayes and Fleet MCF reports, named by data path
- Report headers show what was analysed and the analysis level

## Supporting features

- Guided data-entry wizard — choose your data type (individual, grouped, grouped interval-inspection, or plant cost forecast), paste in raw maintenance data, select the relevant columns, and mark failures vs. suspensions, with validation for missing data
- **Set F / S from a column** (Step 4) — pick the CMMS column that says what kind of job each row was (order type, PM / CM, activity …), tick the values that are repairs, and every row is marked Failure or Suspension in one click; single rows can still be changed, and a warning appears if the data has no such column
- **Analysis level** (Step 2) — the name and location boxes take whatever describes your analysis, and a drop-down records its level: component on one machine, component across like machines, equipment type, or plant-wide. Shown in report headers, with a gentle check before a single-population Weibull or Weibayes analysis of plant-wide data
- **Method checks on the analysis choice form** — live failure/suspension counts with a verdict for Weibull, Crow-AMSAA and Weibayes; warnings if the F/S flags changed after Step 4 or all failures fall at one age; Fleet MCF offered on the date/cycles paths
- Input checks before a run: interval order and overlap checks, numeric and whole-number count checks, and a warning if the working copy of the data no longer matches the CMMS sheet
- Data limit of row 15,000 on input sheets, with a warning instead of silently ignoring rows below it
- Prompts before re-running an analysis or replacing a dataset that already has results, with a shortcut to the existing results
- A Back button on the Crow-AMSAA and Weibull report forms that keeps your data and results
- Main Menu closes any windows still open, so a new analysis always starts clean
- Example dataset included for every data-entry path, plus Weibayes and Fleet MCF examples; the date/cycles examples carry a work-order type column, as a real CMMS export would
- Calendar date-picker for entering dates
- Goodness-of-fit reporting
- Consistent page buttons and form styling throughout

## Relationship to commercial reliability software (brief)

The life-data (Weibull/Exponential/Lognormal), Weibayes, non-parametric survival, Crow-AMSAA and Fleet MCF modules above cover the same statistical ground as the single-population and repairable-system analysis tools in dedicated commercial reliability packages — same estimators (rank regression, MLE, IEC 61164/MIL-HDBK-189, Nelson MCF), same bounding methods (Fisher Matrix, Likelihood Ratio, plus Bayesian and bootstrap options many packages charge extra for), and the same goodness-of-fit diagnostics. What isn't here, and what typically justifies a commercial platform's license cost instead: accelerated life testing and stress-life extrapolation, degradation modeling, system-level reliability block diagrams or fault trees, and formal reliability-growth-with-corrective-actions tracking. If your analysis is single-asset or single-fleet life data, zero-failure demonstration, growth trending, or a comparison of like repairable units, this workbook should get you to the same numbers; for accelerated testing or system-level modeling, use a dedicated platform.
