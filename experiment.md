# M2 Model and Validation Plan

> **Status:** This document describes the planned experiments. Results and verdicts will be recorded separately after the experiments are run.

## Purpose

We will test our two hypotheses with repeatable experiments. Model versions within each hypothesis will use the same walk-forward validation folds. The final test season will be evaluated once after model selection.

We selected different model approaches because the hypotheses ask different questions:

- **H1** asks how game factors relate to winning and how opponent strength changes the comparison between Team A and Team C.
- **H2** asks whether adding preseason rating improves predictions beyond win rate.

H1 and H2 use different final test seasons because tournament outcomes are available through 2025, while regular-season game outcomes are available for 2026.

## Hypothesis 1 — Opponent Strength

> If Team C's higher win rate is mainly due to facing weaker opponents, then Team A will have a higher estimated chance of winning than Team C after we account for opponent strength.

### Data and Row Definition

We plan to use:

- `regular_season_only.csv.gz` for regular-season games and locations.
- `team_strength_preseason.csv` for teams’ preseason ratings.
- `seasons.csv` to organize the data by season.

Each row will represent one team’s perspective on a game. Each game will produce one row for each team:

- The winning team’s row will have an outcome of **win**.
- The losing team’s row will have an outcome of **loss**.
- The location will be represented from the focal team’s perspective: home and away will be reversed for the losing team, while neutral will remain neutral.

The historical games from 2015–2025 will be used for training and validation. The 2026 schedules will provide the shared opponent and location mix for the Team A versus Team C comparison.

**TODO:** Decide how to handle 2019 games, whose cleaned location values are marked unreliable. Do not use `w_loc_original` unless its reliability is confirmed.

### Planned Model: Bayesian Network

We plan to use a Bayesian network because H1 concerns the relationships among a team’s strength, its opponent’s strength, game location, and the game outcome. The network will represent these relationships and estimate the probability of winning a matchup.

Conceptually, the model will include:

- The focal team’s preseason rating.
- The opponent’s preseason rating.
- The game location.
- The game outcome: win or loss.

The focal team’s rating will represent its estimated strength. The opponent’s rating will represent opponent strength. Location will account for where the game was played.

**TODO:** Decide how the Bayesian network will represent numeric preseason ratings.

### Experiment

We will compare two Bayesian-network versions using the same data, season splits, and modeling choices. The only planned difference is whether the opponent’s preseason rating is included.

1. **Baseline version:** Uses the focal team’s preseason rating and game location, but not the opponent’s rating.
2. **Opponent-adjusted version:** Uses the focal team’s preseason rating, the opponent’s preseason rating, and game location.

The opponent-adjusted version will be the primary model for testing H1 because it accounts for opponent strength. The baseline version will serve as a reference for measuring what changes when opponent strength is included.

After validation, we will use the opponent-adjusted model to estimate Team A’s and Team C’s win probabilities over the same mix of opponents and locations from their combined 2026 schedules. We will average each team’s predicted probabilities and compare their adjusted win rates.

### Validation and Data Split

We will use **walk-forward validation**. Each validation fold will hold out one season. The model will train on earlier seasons, and the training data will expand as we move forward. All games from a season, including both team-perspective rows for a game, will remain in the same fold. Both Bayesian-network versions will use the same folds.

- **Initial training:** Seasons 2015–2019.
- **Validation:** We will skip 2020. The first fold will validate on 2021 using 2015–2019 for training. For each next fold, we will add the completed validation season to the training data and validate on the following season through 2025.
- **Final test:** After validation, we will train the opponent-adjusted model on historical data from 2015–2025, excluding 2020, and evaluate it on 2026 once.

We will compare the two versions’ validation Brier scores, their average and spread across seasons, and the training-versus-validation performance gap.

For the final 2026 comparison, we will use Team A’s and Team C’s preseason ratings and the same mix of 2026 opponents and locations to calculate adjusted win-rate estimates. We will not use a 2026 game’s outcome to predict that game.

## Hypothesis 2 — Does Preseason Rating Add Useful Information?

> If preseason ratings tell us something that win rates miss, then using both should give us more accurate estimates of tournament success than using win rate alone.

### Data and Row Definition

We plan to create one row for each active team-season using:

- `team_conferences.csv` or `team_strength_preseason.csv` to identify active team-seasons.
- `regular_season_only.csv.gz` for regular-season records and win rates.
- `team_strength_preseason.csv` for preseason ratings.
- `tournament_results.csv` to determine whether a team won a main-bracket game.
- `tournament_seeds.csv` to identify tournament entrants when needed.
- `teams.csv` for team names and IDs.
- `seasons.csv` to organize the data by season.

The outcome will be whether a team won at least one main-bracket game. Teams that did not enter the tournament will be labeled as having no main-bracket win. Tournament seed will not be used as a model input.

### Planned Model: Logistic Regression

We plan to use logistic regression because the outcome is yes or no, and the model estimates the probability of a team winning at least one main-bracket game.

We will compare two versions of the same model:

1. **Win-rate-only version:** Uses regular-season win rate.
2. **Win-rate-plus-rating version:** Uses regular-season win rate and preseason rating.

Keeping the model type the same will let us test whether adding preseason rating improves the predictions.

### Validation and Data Split

We will use **walk-forward validation**. Each validation fold will hold out one season. The model will train on earlier seasons, and the training data will expand as we move forward. Both logistic-regression versions will use the same folds.

- **Initial training:** Team-season rows from 2015–2019.
- **Validation:** We will skip 2020 because there was no tournament. The first fold will validate on 2021 using 2015–2019 for training. For each next fold, we will add the completed validation season to the training data and validate on the following season through 2024.
- **Final test:** After comparing the two versions on the validation folds and selecting a model, we will train it on historical data from 2015–2024, excluding 2020, and evaluate it on 2025 once.

We will compare the models’ Brier scores for each validation fold, their average and spread across seasons, and the training-versus-validation performance gap. We will use validation results—not the 2025 test result—to decide whether adding preseason rating improves predictions.

After scoring the selected model on the 2025 test, we will generate 2026 probability estimates for Team A and Team C using both model versions. These 2026 estimates will be forecasts only because the dataset has no 2026 tournament outcomes to score against.

## Hypothesis Wording Changes Since M1

The wording of the two hypotheses remained unchanged from M1.