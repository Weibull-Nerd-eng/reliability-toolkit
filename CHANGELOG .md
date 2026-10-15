# Changelog

All notable changes to the **Bi/Tri Weibull & Crow-AMSAA Reliability Analysis Toolkit** are documented here.

## [3.0] - 2026-10-16

Major release on top of REV 2.0. Adds bootstrap confidence intervals, multi-phase Crow-AMSAA with change-point detection, a Duane growth model, MTBF bounds on every Crow path, and a more robust, solver-free calculation core. The workbook grows from 94 to 109 sheets and from about 620 to about 890 VBA procedures, while the file shrinks from about 13.0 MB to about 10.2 MB.

### Added

**Bootstrap confidence intervals** (9 new `Bootstrap ...` sheets)
- Parametric bootstrap intervals for Bi-Weibull and Tri-Weibull on all three data paths (Date/Cycles, Grouped, Grouped Inspection).
- Bootstrap intervals for Crow-AMSAA on all eight Crow paths (Date, Date-F, Cycles, Cycles-F, Grouped, Grouped-F, Inspect, InspectA), reachable from the What-If forms.
- Each run writes a full results sheet and a summary block on the Summary page. Weibull runs also draw a dashed band on the Weibull regression chart.
- Previous bootstrap results are cleared automatically when a new fit or dataset is started, so stale intervals never sit beside a new fit.

**Multi-phase Crow-AMSAA** (8 new `Crow AMSAA ... MultiPhase` sheets)
- Piecewise reliability-growth analysis with a separate Beta/Lambda for each phase, with phase charts, for all eight Crow paths.
- New phase-boundary input form and a phase-count selector.
- Change-point detection and a "Suggest boundary" button that proposes a best split and a secondary split.

**Duane reliability growth model**
- Duane fit (log-log regression) built onto the existing Crow-AMSAA sheets, with its own chart, rate chart and interpretation text box.
- Forecast markers stay synchronised with the What-If inputs.

**Crow-AMSAA MTBF and bounds**
- Cumulative and instantaneous MTBF with Fisher Matrix and Likelihood Ratio bounds on all eight Crow paths. Grouped and Inspect paths previously had no working MTBF.
- One shared bounds engine replaces four near-identical copies of the code.
- What-If forms keep a valid forecast when you leave and return, instead of resetting to placeholders.

**Weibull and cost analysis**
- Beta-binomial 90% confidence band on the Weibull regression chart for all six Bi/Tri paths, replacing straight-line trendlines through bound points. The Grouped path has its own rank-band module.
- Optional planned-replacement and failure-repair downtimes (Tp, Tf) in the replacement-age cost model, for all six cost forms. These were previously fixed at 1 time unit.
- Exact cost optimum found by golden-section search, replacing the whole-unit age grid.
- Cost Pareto and cost uncertainty band.
- User-selectable confidence level for spares stock, with the result shown on all six Spare Parts pages.
- "Reading the chart" tables on the Kaplan-Meier and Actuarial survival pages.

**Workflow and usability**
- New input checks before a run: interval coherence checks for Grouped and Inspect data, numeric and count validation, and a warning if the Date/Cycles working copy no longer matches the CMMS sheet.
- Hard data limit of row 15,000 on input sheets, with a warning instead of silently ignoring rows below it.
- Prompts before re-running an analysis or replacing a dataset that already has results, with a shortcut to the existing results.
- Non-destructive Back button on the Crow-AMSAA selection forms and Date/Cycles Weibull report forms. Before, Back deleted the helper columns that Crow reads and wiped the Weibull results.
- Per-path clearing now removes the Bootstrap, Multi-Phase, Tri/Exponential/Lognormal and Rank Order results for that path only, so nothing from an old dataset is left behind.
- New "Please wait" form for long calculations.

### Changed

- **Plant cost forecast:** the `Output` sheet is now `Maintenance Cost Forecast`. The Annual Plant Read Me is now static text instead of being rebuilt by macro.
- **Grouped Weibull MLE (Bi and Tri) rewritten:** the old routine treated each input row as one unit, so a row with 6 failures counted as 1 failure. It now uses an interval-censored likelihood weighted by the recorded counts, as the Grouped Inspection engine already did.
- **Grouped rank band:** now uses the correct sample size and ranks. The old band used the number of input rows.
- **Tri-model shift parameter (T0 / Gamma):** fitted with a built-in routine instead of Excel Solver, for Tri-Weibull, Tri-Exponential and Tri-Lognormal on Date/Cycles and Grouped, and for the Inspect-path Exponential T0. Suspensions are excluded from the shift fit.
- **Solver removed:** the workbook no longer calls Excel Solver anywhere (11 modules did in REV 2.0). Goal Seek is still used elsewhere.
- **Performance:** chart and curve builders were rewritten as `_Optimized` versions, and duplicated code was consolidated into shared engines (MTBF, Crow bounds, charts). This is a main reason the file is smaller.
- **Naming:** "AMSSA" is now "AMSAA" in 17 sheet names, 8 What-If form names and several procedures. Update any external links or macros that refer to the old names.
- Read Me and UI wording now use "Optimiser" and "minimises".
- The Read Me now links to this changelog.

### Fixed

- **Multi-phase counts:** Phase calculations counted input rows instead of recorded failures on the Grouped and Inspect paths. Every phase's Beta and Lambda was affected, including Phase 1. Counts are now used, and the change-point messages no longer say "failure events" when they mean inspection intervals.
- **Inspect lookup table:** the curve-grid lookup columns on `Grouped Interval Inspections` are now restored automatically if empty.
- **Tri-Weibull Grouped:** the vertical "optimal replacement age" line on the cost chart pointed at the wrong Y values.
- **Tri grid:** the age grid on `TriData with Suspensions group` skipped age 1.
- **Crow What-If:** forms no longer wipe a forecast you entered after a visit to the Graphs page.
- **Weibull bands:** the straight-line trendline band could converge on, or cross, the fit line and extrapolate 30% past the data. The beta-binomial band replaces it.

### Removed

- `Weibull Shape Explanation` sheet (shape explanation content).
- `TriRank order Inspect` sheet (an unimplemented placeholder for interval-adjusted median rank regression).
- Dead and diagnostic code: reference-only and test modules, `ExportAllVBAComponents`, one-off Beta/Lambda and chart-reference repair routines, filter-database and orphaned-name repair routines, and many superseded procedures. These include the per-path `VerticalLines*`, `HorizontalLines*` and `FormatVertLines*` chart helpers and the `X_Scale_*` helpers.
- Old non-optimised Tri-Weibull and Grouped fit routines, replaced by the `_Optimized` versions.
- Duplicate Crow bounds modules (`Module15` to `Module18`), now one shared engine.

### Upgrade notes

- Results will differ from REV 2.0 for **Grouped Weibull fits** (new likelihood), **Tri-model shift parameters** (no Solver), and **Grouped confidence bands** (new ranks).
- Re-run existing analyses rather than relying on saved REV 2.0 output.
- Any macros, links or scripts that reference `Output` or `Crow-AMSSA*` / `Crow AMSSA*` sheets need updating.
- The workbook is still macro-enabled and must be opened with macros on. Windows Excel is recommended.

## [2.0]

Baseline for this changelog. Included Bi/Tri Weibull, Exponential and Lognormal fitting, Crow-AMSAA (Date, Cycles, Grouped, Inspect), Kaplan-Meier and Actuarial survival, cost and spares optimisation, and the plant annual maintenance cost forecast. No earlier changelog existed.
