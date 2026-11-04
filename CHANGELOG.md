## [4.0] - 2026-11-01

Performance, capacity and new-analysis release on top of 3.1. The existing methods now run on site-sized CMMS exports, and three analyses are added on the date / cycles path: a true 3-parameter Weibull MLE, Data Set Comparison and Batch run by class. Input capacity rises from about 15,000 to about 50,000 records per input sheet, and large runs that previously took several minutes, stalled or failed now finish in seconds to a minute or two.

### Added

- True 3-parameter Weibull MLE on the date / cycles Tri-Weibull summary page (rows 64 to 66): Beta, Eta and location found together by maximum likelihood, beside the Tri-Weibull fit as a cross-check. The search takes the highest local maximum and ignores the unbounded rise of the likelihood as the location approaches the first failure. Runs at all data sizes (about 20 seconds at 50,000 records).
- Data Set Comparison (new Data Set Comparison sheet; Compare two data sets on the analysis choice form, date / cycles): split by a column or into before / after a date; MLE fits for Group A, Group B and both together; likelihood-ratio tests for "differ at all" (2 df) and "same Beta" (1 df); Weibull-paper chart; one-page PDF; included in the path report when current. Results are cleared by Clear Data, new data and Start Analysis.
- Batch run by class (new Batch Results sheet; Batch run by class on the analysis choice form, date / cycles): group by any column, defaulting to a header containing "class"; MLE with Likelihood Ratio bounds, B10 and the failure pattern read from Beta's bounds, sorted by B10; landscape PDF; included in the path report when current.
- Larger data capacity: each input sheet now reads data rows 8 to 50,100 (about 50,000 records), up from row 15,000. The row-limit warning moves with it.
- Self-check on the new Crow-AMSAA likelihood calculation: each likelihood ratio bounds run compares the fast calculation with the original one on the current data, and falls back to the original if they ever disagree.

### Changed

- Speed, measured on stress data (20,000 grouped or inspection intervals; 49,990 date / cycles records). Times vary with the PC.
  - Date / cycles 2-parameter Weibull: failed after 52+ minutes, now about 45 seconds; 3-parameter about 1¼ minutes, including the new 3-parameter MLE; Crow-AMSAA about 27 seconds failures only, 36 seconds all data; Clear Data 4½ minutes to about 7 seconds.
  - Grouped 2-parameter Weibull: about 5 minutes to about 1¼ minutes; 3-parameter about 1 minute; Crow-AMSAA failures-only: stalled, now about 14 seconds; all data 1 minute 45 seconds to about 13 seconds; Clear Data 4½ minutes to about 7 seconds.
  - Inspection 2-parameter Weibull: about 2½ minutes to about 1½ minutes; 3-parameter about 1¾ minutes to about 1 minute; Crow-AMSAA about 1 minute to about 14 seconds each; Clear Data 49 to about 17 seconds.
- How: chart-line drawing, goal seeks and the inspection chain no longer recalculate the whole workbook after every cell they write; rank and goodness-of-fit columns are built in memory instead of with look-back formulas; grouped, inspection and Crow-AMSAA likelihoods evaluate each interval end once; root and maximum searches use Newton, Brent and Illinois steps with result caching instead of fixed halvings; the workbook recalculates once before the screen is turned back on (about 11 seconds saved per 50,000-record run).
- Step 4 (date / cycles): Apply to all rows no longer overwrites F / S values changed by hand. Rows that differ from what the rule would set are listed once (Yes keeps them, No sets every row by the rule, Cancel changes nothing); kept rows are shaded, and later Apply clicks recognise them.
- One shared array fitting engine for rank regression, MLE, RBA and Fisher / Likelihood Ratio bounds, used by the new analyses and checked against the sheet calculations.
- Forms consolidated from 66 to 54 (Step 4, Future failures, Spares and the Crow-AMSAA optimiser inputs each merged into one); the two Actuarial modules merged into one.
- The analysis choice form sizes and centres itself for the data path.
- Rank Order S / S Tri are rebuilt without deleting rows.
- The release workbook is saved with the curve lookup tables cleared and the VBA project rebuilt, so the file is smaller.

### Fixed

- Crow-AMSAA bias-correction factor overflowed above about 340 failures; now computed through log-gamma.
- Grouped and inspection loaders read counts formatted as dates as zero; they now read the raw values.
- Summary page ranges that point into the rank-order sheets shrank after repeated runs (caused by row deletes).
- A Summary page cell (AI2) had been saved as text instead of a formula.
- An unguarded case in the likelihood ratio routine.
- Error messages reported error 0 instead of the real error (the error number was reset before it was shown).
- Duane charts: the vertical axis now crosses at the minimum.
- The interval-count column on both grouped input sheets is set to General format.
- Type mismatch in the distribution comparison (2- and 3-parameter) when non-numeric cells appeared in large data sets.
- Excel's "Negative or zero values cannot be plotted on log charts" warning on the grouped and inspection Crow-AMSAA charts: the Actual Data series now start at the first non-zero row.
- The calculated-life marker (purple line) on the six summary pages was drawn before a calculated life was entered.
- Step 4 stopped with an error when Apply to all rows was clicked with no value ticked (all-suspension data, e.g. for Weibayes).
- The Close button on the analysis choice form was cut off for grouped and inspection data.
- Weibayes What-If and Beta forms now open at the top right, clear of the results; tooltips on the analysis choice form buttons corrected.

### Upgrade notes

- Results match 3.1 for all practical purposes. Grouped and inspection Lognormal and Exponential curves can differ in the 7th significant figure because of the faster calculation method.
- Data sets of more than about 10,000 records are best run in 64-bit Excel.
- The curve lookup tables are rebuilt automatically the first time each path is run (this replaces the TEMP_LookupOnDemand_Run step in the 3.1 notes, which no longer exists).
- The workbook is still macro-enabled and must be opened with macros on. Windows Excel is recommended.
