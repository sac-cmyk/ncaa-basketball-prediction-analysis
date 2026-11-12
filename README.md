# The “Unbeatable” Underdog

## Course
CSC 4913/5573: Foundations of Data Model Selection and Validation  
Fall 2026

## Team Members
- Sadiksha Lamsal
- Aaron Michael Edwards
- Samuel Allen Cook

## Topic
Topic 4: Sports Analytics — The “Unbeatable” Underdog

## Problem and Decision
Team C has historically been a weak team, but it is now winning 85% of its games. That is higher than Team A’s 70% win rate, even though Team A has usually been much stronger. Based on these records, a model predicts that Team C will easily win the national championship. We need to find out whether Team C has become a real contender or whether the algorithm is giving too much importance to its win rate.

**Decision:** Should the manager trust the prediction when making decisions involving money, or first check whether Team C’s record tells the whole story?

## Questions and Goals

1. **Do the 2026 regular-season results confirm the reported 85% win rate for Team C and 70% for Team A, and how many games is each percentage based on?**  
   **Goal:** Verify the headline comparison and check whether the percentages are based on similar numbers of games.

2. **How strong were the opponents Team C and Team A faced, and how did their conference schedules compare?**  
   **Goal:** See whether differences in schedule strength could help explain the gap in win rates.

3. **How did each team perform in home, away, and neutral-site games?**  
   **Goal:** Check whether game location could have affected the comparison.

4. **How have Team C’s preseason ratings changed over time, and was its 2026 win rate unusual compared with teams with similar preseason ratings?**
   **Goal:** See whether Team C has been improving and compare its 2026 result with historical preseason expectations.

5. **Across past seasons, how did teams with high regular-season win rates perform in the national tournament?**  
   **Goal:** See whether a high win rate has usually been a good sign of tournament success.

## Repository Guide

#### Data

- `Data/` — Original datasets supplied for the project.
- `cleaned_data/` — Cleaned datasets produced by the cleaning notebook.
- `cleaning_reports/` — Notes on data-quality checks and cleaning decisions.
- `prepared_data/H1/` — Prepared H1 CSV files, including the fold data, fold manifest, game-level data, and shared 2026 comparison scenarios.
- `prepared_data/H2/` — Prepared H2 CSV files, including the team-season data and fold manifest.

#### Notebooks and plans

- `notebooks/cleaning.ipynb` — Documents data cleaning, checks, and row-count changes.
- `notebooks/exploration.ipynb` — Explores the headline win-rate comparison and related patterns.
- `notebooks/feature_engineering_H1.ipynb` and `notebooks/feature_engineering_H2.ipynb` — Prepare the model-ready data and explain the rows, features, outcomes, and splits.
- `notebooks/H1_Model_Experiments.ipynb` — Runs the H1 Bayesian-network comparisons, walk-forward validation, final 2026 test, and Team A–Team C comparison.
- `experiment.md` — Describes the M2 model choices, experiment designs, and validation plans.
- `hypothesis.md` — States the problem, hypotheses, predictions, and planned tests.

#### MLflow tracking

- `mlflow/H{}/mlflow.db` and `mlflow/H{}/mlartifacts/` — Store MLflow run records and logged files. H1 and H2 runs are kept as separate MLflow experiments.

#### Results

- `results/results.md` — M2 results report. H1 results are recorded; H2 has placeholders until its experiment is complete.
- `results/H1/` — H1 validation scores, predictions, figures, verdict, and exported MLflow run table.
- `results/H2/` — H2 results and exported MLflow run table, to be added after its experiment is complete.
- `figures/` — Domain-input graph, hypothesis graphs, and exploration figures.

## AI-Usage

We used AI to help us learn how to work in Jupyter Notebook, clean the datasets, and create visualizations with Matplotlib. We have reviewed the code and results and made the final decisions about the analysis. We also used AI to help us learn how to use MLflow to track model runs and review their results.

