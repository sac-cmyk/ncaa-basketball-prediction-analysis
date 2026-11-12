# M2 Model and Validation Results

This report records the experiments, validation results, final-test results, and hypothesis verdicts for Milestone 2. The experiment designs and model choices are described in [`experiment.md`](../experiment.md).

**Status:** H1 and H2 experiments are recorded below. The hypotheses were unchanged from M1.

## H1 — Opponent Strength

### Experiment

We compared two Bayesian networks. Both used the same walk-forward seasons, training and validation rows, location feature, estimator, prior, and Brier scoring method. The only planned change was adding the opponent's preseason rating group.

| Candidate | Inputs | What changed? | Reason for including it |
|---|---|---|---|
| Baseline | `focal_rating_group`, `focal_location` | Reference model; no opponent rating input | Shows predictions using the focal team's preseason strength and game location. |
| Opponent-adjusted | `focal_rating_group`, `focal_location`, `opponent_rating_group` | Adds `opponent_rating_group` | Tests whether opponent strength adds useful information. |

The target was `outcome` (`win` or `loss`). The model estimated `P(outcome = win)`. `outcome_binary` (1 for a win, 0 for a loss) was used only to calculate Brier scores.

### Walk-forward validation

Each validation fold held out one later season. Both candidates used the same fold each time. Season 2020 was skipped.

| Training seasons | Held-out season |
|---|---:|
| 2015–2019 | 2021 |
| 2015–2019, 2021 | 2022 |
| 2015–2019, 2021–2022 | 2023 |
| 2015–2019, 2021–2023 | 2024 |
| 2015–2019, 2021–2024 | 2025 |

### Validation results

Brier scores are averaged within each held-out season. Lower is better. Spread is the sample standard deviation across the five seasons. The train–validation gap is validation Brier minus training Brier, averaged across folds.

| Model | 2021 | 2022 | 2023 | 2024 | 2025 | Mean validation Brier | Spread (SD) | Mean train–validation gap |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Baseline | 0.2471 | 0.2263 | 0.2227 | 0.2241 | 0.2269 | 0.2295 | 0.0100 | -0.0006 |
| Opponent-adjusted | 0.2218 | 0.1962 | 0.1935 | 0.1973 | 0.1976 | 0.2013 | 0.0116 | -0.0020 |

The opponent-adjusted model had a lower Brier score in all five validation seasons. Its mean score was lower by **0.0282** (about **12.3%** relative to the baseline mean), so it was selected for the final test.

### Final test

The selected opponent-adjusted model was fit on seasons 2015–2019 and 2021–2025. Its reported 2026 Brier score was 0.2003. The notebook rerun logged this test twice; both runs produced the same score, and the test result was not used to choose the model.

### Team A and Team C comparison

The models were fit through 2025 and asked to estimate both teams' win chances on the same **58 opponent-team, opponent-rating-group, and location situations** from their combined 2026 schedules. The scenario file contains no game outcomes, so these are model estimates rather than observed win rates.

| Model | Team A mean predicted chance | Team C mean predicted chance | Difference (A − C) |
|---|---:|---:|---:|
| Baseline | 61.3% | 51.0% | +10.3 percentage points |
| Opponent-adjusted | 71.4% | 51.0% | +20.4 percentage points |

The raw 2026 regular-season win rates were **70.0% for Team A** and **85.7% for Team C**. The shared-situation estimates answer a different question: how the models compare the teams when given the same schedule situations. Adding opponent strength increased Team A's estimated lead by **10.1 percentage points**.

### H1 verdict

**H1 was supported.** On the same 58 opponent-and-location situations, the opponent-adjusted model estimated Team A’s average chance of winning at **71.4%**, compared with **51.0%** for Team C. Adding opponent strength also improved the model’s validation predictions: the mean Brier score fell from **0.2295** for the baseline to **0.2013** for the opponent-adjusted model, which had a lower score in all five validation seasons. The baseline already favored Team A (**61.3% vs. 51.0%**); adding opponent strength made the comparison more informative and increased Team A’s estimated lead from **10.3 to 20.4 percentage points**.

H1 run details and outputs are in [H1 results](H1/). The MLflow database contains **26 runs** because the notebook was rerun; the exported table contains the 13 runs used for this report. The run table, fold-level scores, final-test predictions, comparison predictions, comparison summary, and figures are committed alongside this report.

## H2 — Does Preseason Rating Add Useful Information?

### Experiment design

The outcome was whether a team won at least one main-bracket game. Each row represented one team-season. Non-entrants were labeled as having no main-bracket win. Both candidates used logistic regression and the same season splits; the second candidate added preseason rating.

| Candidate | Input features | Reason for including it |
|---|---|---|
| Win-rate-only | `regular_season_win_rate` | Reference model using regular-season performance. |
| Win-rate-plus-rating | `regular_season_win_rate`, `preseason_rating` | Tests whether preseason rating adds predictive information beyond win rate. |

### Walk-forward validation and final test

Season 2020 was skipped because there was no tournament. Both candidates used the same folds.

| Training seasons | Held-out season |
|---|---:|
| 2015–2019 | 2021 |
| 2015–2019, 2021 | 2022 |
| 2015–2019, 2021–2022 | 2023 |
| 2015–2019, 2021–2023 | 2024 |
| Final fit: 2015–2019, 2021–2024 | Final test: 2025 |

We selected the model using validation results only. The selected model was evaluated on the 2025 test season once. The 2026 estimates for Team A and Team C are forecasts; there are no 2026 tournament outcomes to score.

### Validation results

Brier scores are averaged within each held-out season. The mean is the unweighted average of the four seasonal scores. Spread is the sample standard deviation across seasons. The train–validation gap is validation Brier minus training Brier, averaged across the four folds. Lower Brier is better.

| Model | 2021 | 2022 | 2023 | 2024 | Mean | Spread (SD) | Min–max range | Mean train–validation gap |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Win-rate-only | 0.0668 | 0.0591 | 0.0633 | 0.0633 | 0.0632 | 0.0032 | 0.0077 | -0.0015 |
| Win-rate-plus-rating | 0.0328 | 0.0388 | 0.0361 | 0.0447 | 0.0381 | 0.0050 | 0.0119 | 0.0002 |

### Model selection

**Win-rate-plus-rating** was selected because its mean validation Brier score (**0.0381**) was lower than the win-rate-only score (**0.0632**) by **0.0251**. It had the lower Brier score in **all four** validation seasons. Its seasonal spread was 0.0050, compared with 0.0032 for the win-rate-only model. The mean train–validation gaps were -0.0015 and 0.0002, respectively.

### Final 2025 test and 2026 forecasts

After model selection, the win-rate-plus-rating model was fitted on 2015–2019 and 2021–2024 and evaluated once on the held-out 2025 team-seasons. Its test Brier score was **0.0425** (360 team-seasons). This result was not used to select the model.

Both candidates were then fitted on labeled seasons through 2025 to make the planned 2026 forecasts. These are unscored probabilities, not observed tournament results.

| Model | Team A 2026 forecast probability | Team C 2026 forecast probability |
|---|---:|---:|
| Win-rate-only | 21.8% | 60.2% |
| Win-rate-plus-rating | 80.1% | 4.6% |

### H2 verdict

**H2 was supported.** Adding preseason rating improved the mean validation Brier score from **0.0632** to **0.0381**, with the win-rate-plus-rating model scoring lower in all four validation seasons. Its 2025 final-test Brier score was **0.0425**. The 2026 Team A and Team C probabilities are forecasts only and cannot be checked against tournament outcomes in this dataset.

H2 run details and outputs are in [H2 results](H2/). The exported MLflow table contains **11 runs**: eight walk-forward validation runs, one selected-model final-test run, and two unscored forecast runs. The folder includes the run table, fold-level scores, predictions, forecast files, summary, and figures.

## MLflow run records

The exported run tables list run names, run IDs, parameters, metrics, and artifact locations. H1's local tracking database and logged files are in `mlflow/H1/mlflow.db` and `mlflow/H1/mlartifacts/`; its exported table is [`h1_mlflow_runs.csv`](H1/h1_mlflow_runs.csv). H2's local tracking database and artifacts are in `mlflow/H2/`; its exported table is [`h2_mlflow_runs.csv`](H2/h2_mlflow_runs.csv).

| Hypothesis | Exported run table | Status |
|---|---|---|
| H1 | [`h1_mlflow_runs.csv`](H1/h1_mlflow_runs.csv) | 13 rows exported; 26 runs in the database, including the duplicate set|
| H2 | [`h2_mlflow_runs.csv`](H2/h2_mlflow_runs.csv) | 11 runs |

Run note: The H1 notebook was accidentally rerun, so MLflow contains a duplicate set of the 13 H1 runs. The duplicate runs did not change the model selection or reported results. The 2026 test score was therefore logged twice; the reported Brier score is the same in both runs.
