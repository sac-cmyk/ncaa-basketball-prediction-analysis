# H2 Results — Preseason Rating and Tournament Success

**Execution ID:** `4d84b5b07d`

## Validation results

Both logistic-regression candidates used the same walk-forward folds (2021–2024), scaling pipeline, and Brier score. The only planned difference was adding `preseason_rating` to the second model.

| Model | 2021 Brier | 2022 Brier | 2023 Brier | 2024 Brier | Mean validation Brier | Spread (SD) | Mean train–validation gap |
|---|---:|---:|---:|---:|---:|---:|---:|
| Win-rate-only | 0.0668 | 0.0591 | 0.0633 | 0.0633 | 0.0632 | 0.0032 | -0.0015 |
| Win-rate-plus-rating | 0.0328 | 0.0388 | 0.0361 | 0.0447 | 0.0381 | 0.0050 | 0.0002 |

**Model selected from validation:** Win-rate-plus-rating. It had the lower mean Brier score; an exact tie defaults to the simpler win-rate-only candidate.
**Season-by-season check:** Win-rate-plus-rating had the lower Brier score in 4 of 4 validation seasons.

## Final test

The selected model was refit on 2015–2019 and 2021–2024 and scored once on the held-out 2025 team-seasons. Its final-test Brier score was **0.0425** (n=360). The test score was not used to select the model.

## 2026 forecasts (not scored)

Both models were refit on labeled seasons through 2025. Because 2026 tournament outcomes are unavailable, these probabilities are forecasts and have no Brier score.

| Model | Team A forecast probability | Team C forecast probability |
|---|---:|---:|
| Win-rate-only | 0.2182 | 0.6024 |
| Win-rate-plus-rating | 0.8007 | 0.0462 |

## H2 verdict

Supported: adding preseason rating produced the lower mean validation Brier score.
The mean validation Brier scores were 0.0632 for win-rate-only (SD 0.0032) and 0.0381 for win-rate-plus-rating (SD 0.0050).

## Files

- `h2_validation_results.csv` and `h2_validation_fold_scores.csv` — validation scores, spread, and train–validation gaps.
- `h2_validation_predictions.csv` and `h2_final_test_predictions.csv` — saved team-season predictions and actual labels.
- `h2_2026_forecasts.csv` and `h2_2026_team_a_team_c_forecasts.csv` — unscored 2026 forecasts.
- `h2_mlflow_runs.csv` — exported MLflow run names, IDs, parameters, metrics, and artifact locations.
- `figures/` — validation and forecast plots.
- `mlflow/H2/` — local MLflow database and artifacts; these are reproducibility files, while the exported CSV run table is the compact report artifact.