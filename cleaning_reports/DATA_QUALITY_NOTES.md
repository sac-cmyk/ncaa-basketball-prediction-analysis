# Data quality notes

- Exact duplicates removed: 326 results rows and 522 box-score rows. Other files: see row_counts.csv.
- Verified score reversals repaired: 331 games; both reconstructed box scores support each correction. IDs are unchanged. See score_corrections.csv.
- Existing missing FGA values: 2645; after masking inconsistent FGA: 2651. No imputation. See invalid_statistics.csv and box_score_missing_values.csv.
- Impossible count cells made missing: 12. This includes negative rebounds and inconsistent attempts. Values cannot be reliably reconstructed.
- All 5586 games in 2019 retain their outcomes but have unknown w_loc. Original labels are in w_loc_original; location_reliable marks reliable seasons. Venue analyses must exclude unknown locations.
- fga_consistency_issue flags attempts that conflict with other shooting counts. Do not interpret missing FGA as zero; report the usable denominator for shooting features.
- Conference tournaments remain in regular_season_results.csv.gz, flagged by is_conference_tourney. They are excluded only in regular_season_only.csv.gz. Do not append conference_tourney_games to results.
- Blank brief_alias values are intentional for teams other than A and C.
- 2020 had no national tournament. Missing seeds/results for it are structural. 2021 had fewer games and low attendance, which can affect venue comparisons.
- The supplied file has 0 seeds for 2026. The 2026 national tournament results are not yet available.
- Preseason ratings are fixed estimates, not final season rankings. Use team ID and season for joins, including conference membership.
- Corrected score margins and the headline regular-season restriction have different effects. See headline_before_after.csv and the notebook's margin output. Venue sensitivity is a sample comparison, not recovery of true 2019 locations.

All corrections are reproducible in `notebooks/cleaning.ipynb`. Original files are not overwritten.
