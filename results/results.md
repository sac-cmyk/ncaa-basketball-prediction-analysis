# M2 Model and Validation Results

This report records the experiments, validation results, final-test results, and hypothesis verdicts for Milestone 2. The experiment designs and model choices are described in [`experiment.md`](../experiment.md).

**Status:** H1 results are recorded below. H2 is still in progress; its marked TODOs must be completed after its models are run and reviewed.

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

The selected opponent-adjusted model was fit on seasons 2015–2019 and 2021–2025, then scored once on the held-out 2026 games. Its 2026 Brier score was **0.2003**. This test score was not used to choose the model.

### Team A and Team C comparison

The models were fit through 2025 and asked to estimate both teams' win chances on the same **58 opponent-team, opponent-rating-group, and location situations** from their combined 2026 schedules. The scenario file contains no game outcomes, so these are model estimates rather than observed win rates.

| Model | Team A mean predicted chance | Team C mean predicted chance | Difference (A − C) |
|---|---:|---:|---:|
| Baseline | 61.3% | 51.0% | +10.3 percentage points |
| Opponent-adjusted | 71.4% | 51.0% | +20.4 percentage points |

The raw 2026 regular-season win rates were **70.0% for Team A** and **85.7% for Team C**. The shared-situation estimates answer a different question: how the models compare the teams when given the same schedule situations. Adding opponent strength increased Team A's estimated lead by **10.1 percentage points**.

### H1 verdict

**H1 was supported.** On the same 58 opponent-and-location situations, the opponent-adjusted model estimated Team A’s average chance of winning at **71.4%**, compared with **51.0%** for Team C. Adding opponent strength also improved the model’s validation predictions: the mean Brier score fell from **0.2295** for the baseline to **0.2013** for the opponent-adjusted model, which had a lower score in all five validation seasons. The baseline already favored Team A (**61.3% vs. 51.0%**); adding opponent strength made the comparison more informative and increased Team A’s estimated lead from **10.3 to 20.4 percentage points**.

H1 run details and outputs are in [`H1 results`](H1/). The exported MLflow table contains **13 runs**. The run table, fold-level scores, final-test predictions, comparison predictions, comparison summary, and figures are committed alongside this report.

## H2 — Does Preseason Rating Add Useful Information?

**Status:** TODO — the experiment is still being run. Replace every TODO below with the team's checked results before the M2 tag.

### Experiment design

The outcome is whether a team won at least one main-bracket game. Each row is one team-season. Non-entrants are labeled as no main-bracket win. Both candidates use logistic regression and the same season splits; the second candidate adds preseason rating.

| Candidate | Input features | Reason for including it |
|---|---|---|
| Win-rate-only | `regular_season_win_rate` | Reference model using regular-season performance. |
| Win-rate-plus-rating | `regular_season_win_rate`, `preseason_rating` | Tests whether preseason rating adds predictive information beyond win rate. |

### Walk-forward validation and final test

Season 2020 is skipped because there was no tournament. Both candidates must use the same folds.

| Training seasons | Held-out season |
|---|---:|
| 2015–2019 | 2021 |
| 2015–2019, 2021 | 2022 |
| 2015–2019, 2021–2022 | 2023 |
| 2015–2019, 2021–2023 | 2024 |
| Final fit: 2015–2019, 2021–2024 | Final test: 2025 |

The model will be selected using validation results only. The selected model will be evaluated on the 2025 test season once. The 2026 estimates for Team A and Team C are forecasts; there are no 2026 tournament outcomes to score.

### Validation results

Report each candidate's Brier score for every validation season. Include the mean, sample standard deviation across seasons, min–max range, and mean train–validation gap. Lower Brier is better.

| Model | 2021 | 2022 | 2023 | 2024 | Mean | Spread (SD) | Min–max range | Mean train–validation gap |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Win-rate-only | TODO | TODO | TODO | TODO | TODO | TODO | TODO | TODO |
| Win-rate-plus-rating | TODO | TODO | TODO | TODO | TODO | TODO | TODO | TODO |

**Model selection:** TODO — state which model had the lower mean validation Brier and whether the result was consistent across the four folds.

### Final 2025 test and 2026 forecasts

**Selected model's 2025 test Brier:** TODO — report once, after model selection.

| Model | Team A 2026 forecast probability | Team C 2026 forecast probability |
|---|---:|---:|
| Win-rate-only | TODO | TODO |
| Win-rate-plus-rating | TODO | TODO |

These are forecasts, not scored results. Do not calculate a 2026 Brier score without the tournament outcomes.

### H2 verdict

**Verdict:** TODO — supported, rejected, revised, or unresolved.

**Evidence:** TODO — compare the candidates' validation Brier scores, spread across folds, train–validation gaps, and the 2025 test score. State whether adding preseason rating improved prediction.

## MLflow run records

The exported run tables are the readable summaries of the experiments. Each row identifies a run and includes its run ID, parameters, metrics, and artifact reference. H1's local tracking database and logged files are in `prepared_data/H1/mlflow.db` and `prepared_data/H1/mlartifacts/`; its exported table is [`h1_mlflow_runs.csv`](H1/h1_mlflow_runs.csv). Add H2's tracking records and exported table after its experiment is complete.

| Hypothesis | Exported run table | Status |
|---|---|---|
| H1 | [`results/H1/h1_mlflow_runs.csv`](H1/h1_mlflow_runs.csv) | Recorded: 13 runs |
| H2 | `results/H2/h2_mlflow_runs.csv` | TODO — export and commit after running H2 |

## M2 completion checklist

- [x] H1 candidates, fixed settings, and one-factor comparison documented.
- [x] H1 walk-forward validation results, spread, train–validation gap, selected model, and one-time 2026 test score recorded.
- [x] H1 shared-situation comparison and qualified verdict recorded.
- [x] H1 MLflow run table and experiment outputs included.
- [ ] H2 candidates and experiment rationale reviewed against the plan.
- [ ] H2 walk-forward validation results, spread, and train–validation gaps recorded for both candidates.
- [ ] H2 model selected from validation results, then scored once on the 2025 test set.
- [ ] H2 forecast estimates and verdict recorded; H2 MLflow run table exported and committed.
- [x] Hypothesis wording changes since M1 recorded in [`experiment.md`](../experiment.md) as unchanged.
- [ ] Confirm the complete repository is committed and tag it `M2` by the end of November 11, 2026.
