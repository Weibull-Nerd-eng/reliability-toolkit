## [4.0] - 2026-11-01

Performance and capacity release on top of 3.1. No new analyses; the same methods now run on site-sized CMMS exports. Input capacity rises from about 15,000 to about 50,000 records per input sheet, and large runs that previously took several minutes, stalled or failed now finish in seconds to a couple of minutes.

### Added

- Larger data capacity: each input sheet now reads data rows 8 to 50,100 (about 50,000 records), up from row 15,000. The row-limit warning moves with it.
- Self-check on the new Crow-AMSAA likelihood calculation: each likelihood ratio bounds run compares the fast calculation with the original one on the current data, and falls back to the original if they ever disagree.

### Changed

- Speed, measured on stress data (20,000 grouped or inspection intervals; 49,990 date / cycles records). Times vary with the PC.
  - Date / cycles 2-parameter Weibull: failed after 52+ minutes, now about 1½ minutes; 3-parameter about 2 minutes; Clear Data 4½ minutes to 9 seconds.
  - Grouped 2- / 3-parameter Weibull: about 5 minutes to under 2 minutes; Crow-AMSAA failures-only: stalled, now about 15 seconds; all data 1 minute 45 seconds to about 12 seconds; Clear Data 4½ minutes to about 8 seconds.
  - Inspection 2-parameter Weibull: about 2½ minutes to 45 seconds – 1½ minutes; 3-parameter about 1¾ minutes to about 1 minute; Crow-AMSAA about 1 minute to about 15 seconds each; Clear Data 49 to 20 seconds.
- How: chart-line drawing, goal seeks and the inspection chain no longer recalculate the whole workbook after every cell they write; rank and goodness-of-fit columns are built in memory instead of with look-back formulas; grouped, inspection and Crow-AMSAA likelihoods evaluate each interval end once; root and maximum searches use Newton, Brent and Illinois steps with result caching instead of fixed halvings.
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

### Upgrade notes

- Results match 3.1 for all practical purposes. Grouped and inspection Lognormal and Exponential curves can differ in the 7th significant figure because of the faster calculation method.
- Data sets of more than about 10,000 records are best run in 64-bit Excel.
- The curve lookup tables are rebuilt automatically the first time each path is run (this replaces the TEMP_LookupOnDemand_Run step in the 3.1 notes, which no longer exists).
- The workbook is still macro-enabled and must be opened with macros on. Windows Excel is recommended.
