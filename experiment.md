# M2 Model and Validation Plan

> This document records the experiment designs and choices for M2. Numerical results, MLflow records, and hypothesis verdicts are reported in [`results/results.md`](results/results.md).

## Purpose

We designed repeatable experiments to test our two hypotheses. Within each hypothesis, candidate models use the same walk-forward validation folds so their results can be compared fairly. The selected model is evaluated on its final test season once, after model selection.

We chose different model types because the hypotheses ask different questions:

- **H1** asks whether adding opponent strength improves game-win predictions and how it changes the Team A–Team C comparison.
- **H2** asks whether adding preseason rating to regular-season win rate improves predictions of tournament success.

H1 and H2 use different final test seasons because regular-season game results are available through 2026, while tournament outcomes are available through 2025.

## Hypothesis 1 — Opponent Strength

> If Team C's higher win rate is mainly due to facing weaker opponents, then Team A will have a higher estimated chance of winning than Team C after we account for opponent strength.

### Data and Row Definition

The H1 preparation uses:

- `regular_season_only.csv.gz` for regular-season games and locations.
- `team_strength_preseason.csv` for teams’ preseason ratings.
- `seasons.csv` to organize the data by season.

Each row represents one team’s perspective on one game. Each game produces two rows:

- The winning team’s row has `outcome = win`.
- The losing team’s row has `outcome = loss`.
- The location is recorded from that row’s team perspective. Home and away are reversed for the losing team; neutral remains neutral.
- `outcome_binary` is 1 for a win and 0 for a loss. It is used to calculate Brier scores, not as a model input.

The prepared H1 files are in `prepared_data/H1/`. They include the model-ready fold data, fold manifest, game-level data, and shared 2026 comparison scenarios.

The cleaned location value for 2019 games was unreliable, so it was recorded as `Unknown` and kept as its own location category. The `w_loc_original` field was not used.

Preseason ratings were numerical, so they were grouped as **low, medium, or high** for the discrete Bayesian network. The rating cutoffs are recorded in `h1_fold_manifest.csv`.

### Model: Bayesian Network

H1 uses a discrete Bayesian network from **pgmpy** because it estimates the chance of winning from categorical information about a team, its opponent, and game location.

The model predicts `outcome` (`win` or `loss`) and estimates the probability of a win. Its inputs are:

| Model | Input columns |
|---|---|
| Baseline | `focal_rating_group`, `focal_location` |
| Opponent-adjusted | `focal_rating_group`, `focal_location`, `opponent_rating_group` |

The **focal team** is the team represented by a row. The opponent is the other team in that game. The baseline uses the focal team’s preseason strength group and location. The opponent-adjusted model adds the opponent’s preseason strength group.

Both versions use the same Bayesian estimator, BDeu prior, equivalent sample size of 10, and exact inference by variable elimination. The network structure is fixed, with each input pointing to `outcome`.

### Experiment

We compare two Bayesian-network versions. Both use the same training data, validation folds, target, Brier scoring method, and model-fitting settings. The only planned difference is whether `opponent_rating_group` is included.

1. **Baseline:** Uses the focal team’s rating group and location.
2. **Opponent-adjusted:** Uses the focal team’s rating group, location, and the opponent’s rating group.

This comparison tests whether adding opponent strength improves predictions beyond the baseline inputs.

For the separate Team A–Team C comparison, both models estimate win chances for both teams using the same 58 opponent-team and location situations from their combined 2026 schedules. In each situation, both teams are evaluated against the same opponent and at the same location. The scenario file contains no game outcomes.

### Validation and Data Split

We use **walk-forward validation**. Each fold holds out one season. The models train on earlier seasons, then the training data expands for the next fold. All games from a season stay together. Both models use the same folds, and 2020 is skipped.

| Training seasons | Validation season |
|---|---:|
| 2015–2019 | 2021 |
| 2015–2019 and 2021 | 2022 |
| 2015–2019 and 2021–2022 | 2023 |
| 2015–2019 and 2021–2023 | 2024 |
| 2015–2019 and 2021–2024 | 2025 |

For each model, we report the Brier score for every validation season, the mean and spread across seasons, and the training-versus-validation gap. A lower Brier score means the predicted probabilities were closer to the game outcomes.

The model selected using validation results is fit on the available seasons through 2025, excluding 2020, and scored on the 2026 games once. The 2026 game outcomes are used only for this final test score—not for the Team A–Team C comparison.

For the Team A–Team C comparison, both model versions are fit through 2025. For each model, we average the predicted win chances across the same 58 situations and compare Team A’s average with Team C’s. These are model estimates, not actual win rates.

## Hypothesis 2 — Does Preseason Rating Add Useful Information?

> If preseason ratings tell us something that win rates miss, then using both should give us more accurate estimates of tournament success than using win rate alone.

### Data and Row Definition

The H2 preparation creates one row for each active team-season using:

- `regular_season_only.csv.gz` for regular-season games and win rates.
- `team_strength_preseason.csv` for preseason ratings.
- `tournament_results.csv` to determine whether a team won a main-bracket game.
- `tournament_seeds.csv` to identify tournament entrants when needed.
- `teams.csv` for team names and IDs.
- `team_conferences.csv` or `team_strength_preseason.csv` to identify active team-seasons.
- `seasons.csv` to organize the data by season.

The outcome is whether a team won at least one main-bracket game. A team that did not enter the tournament is labeled as having no main-bracket win. A First Four win does not count as a main-bracket win. Tournament seed is not used as a model input.

The prepared H2 files are in `prepared_data/H2/`.

### Model: Logistic Regression

H2 uses logistic regression because the outcome is yes or no and the model estimates the probability of a team winning at least one main-bracket game.

We compare two versions of the same model:

1. **Win-rate-only:** Uses `regular_season_win_rate`.
2. **Win-rate-plus-rating:** Uses `regular_season_win_rate` and `preseason_rating`.

The model type, folds, scoring method, and fitting settings stay the same. The planned difference is whether `preseason_rating` is included.

### Validation and Data Split

We use **walk-forward validation**. Each fold holds out one season, and the training data expands as we move forward. Both logistic-regression versions use the same folds. Season 2020 is skipped because there was no tournament.

| Training seasons | Validation season |
|---|---:|
| 2015–2019 | 2021 |
| 2015–2019 and 2021 | 2022 |
| 2015–2019 and 2021–2022 | 2023 |
| 2015–2019 and 2021–2023 | 2024 |

After comparing validation results and selecting a model, we train it on 2015–2019 and 2021–2024, then evaluate it on the 2025 test season once. We use validation results—not the 2025 test result—to decide whether adding preseason rating improves predictions.

We report each model’s Brier score for every validation season, the mean and spread across seasons, and the training-versus-validation gap.

After scoring the selected model on the 2025 test, we generate 2026 probability forecasts for Team A and Team C using both model versions. These are forecasts only; there are no 2026 tournament outcomes available to score them.

## Hypothesis Wording Changes Since M1

The wording of both hypotheses remained unchanged from M1.