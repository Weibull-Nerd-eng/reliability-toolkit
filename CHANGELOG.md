# Changelog

All notable changes to the Bi/Tri Weibull & Crow-AMSAA Reliability Analysis Toolkit are documented here.

## [3.1] - 2026-10-25

Feature release on top of 3.0. Adds Weibayes zero-failure analysis on every data path, Fleet MCF for comparing repairable units, one-click Failure / Suspension marking from the CMMS work-order type, an analysis-level setting, method checks on the analysis choice form, and cleaner one-page report printing. The workbook grows from 109 to 111 sheets, while the file shrinks from about 10.2 MB to about 7.0 MB (about 5.9 MB when saved with the curve lookup tables cleared — see Upgrade notes).

### Added

#### Weibayes zero-failure analysis (new Weibayes sheet)

- Life claims when there are no failures, or only one or two: one-sided lower bounds on Eta and on the B-life, reliability at a mission time, and an upper bound and Weibayes estimate when there are failures.
- Available on all data paths (Date, Cycles, Grouped, Inspect) from a new Weibayes button on the analysis choice form. Reads the Failure / Suspension flags live from the input sheet, exactly as they are.
- Beta defaults to the last Weibull fit on that path; a Beta sensitivity table shows the least favourable case across the credible range.
- Demonstration planning: whether a target B-life is already demonstrated, the extra failure-free running each unit needs, or the test time for new units.
- New What-If form for the target and test units, with guidance on realistic targets from the data and live previews.
- Weibull plot (Weibayes line with lower / upper bounds, failures and running units) and reliability curve with mission time, oldest unit and target marked. Interval data plots one point per inspection, at the interval end.
- One-page PDF report, named by data path, with the path shown in the title, file name and footer.
- A Weibayes example for each data path.

#### Fleet MCF for repairable units (new Fleet MCF sheet, Date and Cycles paths)

- Nelson mean cumulative function with Lawless-Nadeau robust confidence bounds.
- Fleet power-law (multiple-system Crow) Beta with bounds and a rising / falling / no-clear-trend verdict.
- Units ranked by observed against expected repairs for their own observation time, with HIGH / low flags from a Poisson test.
- New column-mapping form: unit, event date or meter reading, and work-order type are chosen from the actual row-7 headers, with tick boxes for the values that mean a repair. Choices are remembered by header name, so a re-ordered CMMS export is still recognised. Unit names are matched with spaces trimmed and case ignored.
- Observation window per unit (earliest date, each unit's first record or a fixed date; analysis date, each unit's last record or a fixed date).
- MCF chart with the top-ranked units overlaid, an event plot of every unit, and a one-page PDF report.
- Reachable from a new button on the analysis choice form (Date / Cycles only) and from the Crow-AMSAA menu.
- A six-pump example with typical CMMS headers (Asset Tag, Asset Type, Date Reported, Assigned To, Work Order Description, Order type, Total Cost).

#### Data entry

- Set F / S from a column on both Step 4 forms: pick the work-order type column, tick the repair values, and every row is marked F or S in one click. The likely column is found automatically from header words and PM / CM-style values, and a warning appears if the data has none. The choice is shared with Fleet MCF.
- Analysis level on all three Step 2 forms: component on one machine, component across like machines, equipment type, or plant-wide. The name and location boxes stay free text, with a hint and examples. The level is shown in report headers.
- Method checks on the analysis choice form: live failure / suspension counts with a verdict for Weibull, Crow-AMSAA and Weibayes; warnings when F / S changed after Step 4, when there are few failures, or when all failures fall at one age or interval; a level note, and a gentle check before Weibull or Weibayes on plant-wide data.
- Main Menu now closes any windows still open, so a new analysis always starts clean.
- The date / cycles examples gain an Order type column, as a real CMMS export would have.

#### Reporting

- Path reports: every page printed as large as fits on one page, in whichever orientation suits it.
- Report headers show the analysis level after the analysis name.

### Changed

- Page buttons on every sheet use one house style, and each button is grouped with its backing bar so the two always move together.
- All user forms use Calibri, with dark-blue headings and the same pale-yellow input boxes as the sheets.
- The curve-grid lookup tables (columns EB:EE on the six data sheets) are built on demand when a chart needs them, instead of being stored in the file.
- Formatting removed from the pure calculation sheets; PDF and user-facing sheets are unchanged.
- Crow-AMSAA date and Crow-AMSAA dateF sheets rebuilt as fresh sheets.
- The yellow input shading on the eight Crow-AMSAA Cost Opt and Spares Opt sheets is removed; those inputs are set on the optimisation forms.
- Read Me updated for Weibayes, Fleet MCF, the analysis level and Set F / S from a column, and the disclaimer now describes the plant forecast's uncertainty band.

### Fixed

- Weibayes reliability at mission time: an operator-precedence error in the sheet formula returned values above 100%.
- Weibayes and the F / S data checks now read the input sheet directly, so they no longer disagree with what the user has just changed.
- Crow-AMSAA dateF stored all 1,048,576 rows (a zero default row height with every row hidden by default) and Crow-AMSAA date held 65,536 legacy rows from the old .xls format; this was about 2.7 MB of the file.
- Report pages: cut-off text (for example the trend verdict and long labels) now shrinks to fit; the Duane interpretation box wraps inside its chart; small Crow-AMSAA MTBF values now show enough decimals instead of 0.01, and fractional values such as forecast days are no longer rounded to whole numbers.
- Date-path Cost, Spare Parts and Future pages now have full titles, matching the Grouped and Inspect paths.
- Bootstrap detail pages no longer print internal sheet!cell references in their captions.
- Chart data labels on marker lines no longer overlap the axis labels or the curves.
- Button bars that had drifted from their buttons were realigned (32 of 160), and a button without a bar was given one.

### Upgrade notes

- Saved 3.0 results are unaffected, but re-run Weibayes and the path reports to pick up the new layout and headers.
- Fleet MCF and the one-click F / S setting work from the user's own column headers; no fixed CMMS layout is needed.
- New hidden workbook names store the analysis level (AL_Date, AL_Grouped, AL_Inspect) and the Set F / S choice (ET_TypeHeader, ET_RepairVals). Don't delete them.
- The curve lookup tables come back on a data sheet once its path has been run (about 0.2–0.3 MB each). To hand out the smallest file, run TEMP_LookupOnDemand_Run before saving the release copy.
- The workbook is still macro-enabled and must be opened with macros on. Windows Excel is recommended.

## [3.0] - 2026-10-16

Major release on top of REV 2.0. Adds bootstrap confidence intervals, multi-phase Crow-AMSAA with change-point detection, a Duane growth model, MTBF bounds on every Crow path, and a more robust, solver-free calculation core. The workbook grows from 94 to 109 sheets and from about 620 to about 890 VBA procedures, while the file shrinks from about 13.0 MB to about 10.2 MB.

### Added

#### Bootstrap confidence intervals (9 new Bootstrap ... sheets)

- Parametric bootstrap intervals for Bi-Weibull and Tri-Weibull on all three data paths (Date/Cycles, Grouped, Grouped Inspection).
- Bootstrap intervals for Crow-AMSAA on all eight Crow paths (Date, Date-F, Cycles, Cycles-F, Grouped, Grouped-F, Inspect, InspectA), reachable from the What-If forms.
- Each run writes a full results sheet and a summary block on the Summary page. Weibull runs also draw a dashed band on the Weibull regression chart.
- Previous bootstrap results are cleared automatically when a new fit or dataset is started, so stale intervals never sit beside a new fit.

#### Multi-phase Crow-AMSAA (8 new Crow AMSAA ... MultiPhase sheets)

- Piecewise reliability-growth analysis with a separate Beta/Lambda for each phase, with phase charts, for all eight Crow paths.
- New phase-boundary input form and a phase-count selector.
- Change-point detection and a "Suggest boundary" button that proposes a best split and a secondary split.

#### Duane reliability growth model

- Duane fit (log-log regression) built onto the existing Crow-AMSAA sheets, with its own chart, rate chart and interpretation text box.
- Forecast markers stay synchronised with the What-If inputs.

#### Crow-AMSAA MTBF and bounds

- Cumulative and instantaneous MTBF with Fisher Matrix and Likelihood Ratio bounds on all eight Crow paths. Grouped and Inspect paths previously had no working MTBF.
- One shared bounds engine replaces four near-identical copies of the code.
- What-If forms keep a valid forecast when you leave and return, instead of resetting to placeholders.

#### Weibull and cost analysis

- Beta-binomial 90% confidence band on the Weibull regression chart for all six Bi/Tri paths, replacing straight-line trendlines through bound points. The Grouped path has its own rank-band module.
- Optional planned-replacement and failure-repair downtimes (Tp, Tf) in the replacement-age cost model, for all six cost forms. These were previously fixed at 1 time unit.
- Exact cost optimum found by golden-section search, replacing the whole-unit age grid.
- Cost Pareto and cost uncertainty band.
- User-selectable confidence level for spares stock, with the result shown on all six Spare Parts pages.
- "Reading the chart" tables on the Kaplan-Meier and Actuarial survival pages.

#### Workflow and usability

- New input checks before a run: interval coherence checks for Grouped and Inspect data, numeric and count validation, and a warning if the Date/Cycles working copy no longer matches the CMMS sheet.
- Hard data limit of row 15,000 on input sheets, with a warning instead of silently ignoring rows below it.
- Prompts before re-running an analysis or replacing a dataset that already has results, with a shortcut to the existing results.
- Non-destructive Back button on the Crow-AMSAA selection forms and Date/Cycles Weibull report forms. Before, Back deleted the helper columns that Crow reads and wiped the Weibull results.
- Per-path clearing now removes the Bootstrap, Multi-Phase, Tri/Exponential/Lognormal and Rank Order results for that path only, so nothing from an old dataset is left behind.
- New "Please wait" form for long calculations.

### Changed

- Plant cost forecast: the Output sheet is now Maintenance Cost Forecast. The Annual Plant Read Me is now static text instead of being rebuilt by macro.
- Grouped Weibull MLE (Bi and Tri) rewritten: the old routine treated each input row as one unit, so a row with 6 failures counted as 1 failure. It now uses an interval-censored likelihood weighted by the recorded counts, as the Grouped Inspection engine already did.
- Grouped rank band: now uses the correct sample size and ranks. The old band used the number of input rows.
- Tri-model shift parameter (T0 / Gamma): fitted with a built-in routine instead of Excel Solver, for Tri-Weibull, Tri-Exponential and Tri-Lognormal on Date/Cycles and Grouped, and for the Inspect-path Exponential T0. Suspensions are excluded from the shift fit.
- Solver removed: the workbook no longer calls Excel Solver anywhere (11 modules did in REV 2.0). Goal Seek is still used elsewhere.
- Performance: chart and curve builders were rewritten as _Optimized versions, and duplicated code was consolidated into shared engines (MTBF, Crow bounds, charts). This is a main reason the file is smaller.
- Naming: "AMSSA" is now "AMSAA" in 17 sheet names, 8 What-If form names and several procedures. Update any external links or macros that refer to the old names.
- Read Me and UI wording now use "Optimiser" and "minimises".
- The Read Me now links to this changelog.

### Fixed

- Multi-phase counts: Phase calculations counted input rows instead of recorded failures on the Grouped and Inspect paths. Every phase's Beta and Lambda was affected, including Phase 1. Counts are now used, and the change-point messages no longer say "failure events" when they mean inspection intervals.
- Inspect lookup table: the curve-grid lookup columns on Grouped Interval Inspections are now restored automatically if empty.
- Tri-Weibull Grouped: the vertical "optimal replacement age" line on the cost chart pointed at the wrong Y values.
- Tri grid: the age grid on TriData with Suspensions group skipped age 1.
- Crow What-If: forms no longer wipe a forecast you entered after a visit to the Graphs page.
- Weibull bands: the straight-line trendline band could converge on, or cross, the fit line and extrapolate 30% past the data. The beta-binomial band replaces it.

### Removed

- Weibull Shape Explanation sheet (shape explanation content).
- TriRank order Inspect sheet (an unimplemented placeholder for interval-adjusted median rank regression).
- Dead and diagnostic code: reference-only and test modules, ExportAllVBAComponents, one-off Beta/Lambda and chart-reference repair routines, filter-database and orphaned-name repair routines, and many superseded procedures. These include the per-path VerticalLines*, HorizontalLines* and FormatVertLines* chart helpers and the X_Scale_* helpers.
- Old non-optimised Tri-Weibull and Grouped fit routines, replaced by the _Optimized versions.
- Duplicate Crow bounds modules (Module15 to Module18), now one shared engine.

### Upgrade notes

- Results will differ from REV 2.0 for Grouped Weibull fits (new likelihood), Tri-model shift parameters (no Solver), and Grouped confidence bands (new ranks).
- Re-run existing analyses rather than relying on saved REV 2.0 output.
- Any macros, links or scripts that reference Output or Crow-AMSSA* / Crow AMSSA* sheets need updating.
- The workbook is still macro-enabled and must be opened with macros on. Windows Excel is recommended.

## [2.0]

Baseline for this changelog. Included Bi/Tri Weibull, Exponential and Lognormal fitting, Crow-AMSAA (Date, Cycles, Grouped, Inspect), Kaplan-Meier and Actuarial survival, cost and spares optimisation, and the plant annual maintenance cost forecast. No earlier changelog exists.
