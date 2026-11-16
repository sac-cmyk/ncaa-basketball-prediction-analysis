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
- `notebooks/H2_Model_Experiments.ipynb` — Runs the H2 logistic regression comparison, walk-forward validation, 2025 final test, and unscored 2026 forecasts for Team A and Team C.
- `experiment.md` — Describes the M2 model choices, experiment designs, and validation plans.
- `hypothesis.md` — States the problem, hypotheses, predictions, and planned tests.

#### MLflow tracking

- `mlflow/H{}/mlflow.db` and `mlflow/H{}/mlartifacts/` — Store MLflow run records and logged files. H1 and H2 runs are kept as separate MLflow experiments.

#### Results

- `results/results.md` — M2 results report. 
- `results/H1/` — H1 validation scores, predictions, figures, verdict, and exported MLflow run table.
- `results/H2/` — H2 validation scores, predictions, forecasts, figures, summary, and exported MLflow run table.
- `figures/` — Domain-input graph, hypothesis graphs, and exploration figures.

## AI-Usage

We used AI at different stages of the project. It helped us learn Python and Jupyter syntax, understand how to prepare data and build features, and figure out how to use Bayesian networks and logistic regression in our notebooks. It also helped us understand walk-forward validation and Brier scores, create Matplotlib visualizations, and set up MLflow to record model runs and results. We reviewed the code and checked the results ourselves; our team made the final decisions about the experiments and conclusions.

## Conclusions and Recommendation

### H1 — Opponent strength

**Verdict: Supported in the predicted direction.** On the same 58 opponent-and-location situations, the opponent-adjusted model estimated Team A’s average win chance at **71.4%** and Team C’s at **51.0%**. Its mean validation Brier score was **0.2013**, compared with **0.2295** for the baseline.

The baseline also favored Team A, so these results do not show that opponent strength alone explains Team C’s higher raw 2026 win rate. The estimates are model predictions, not observed win rates.

### H2 — Preseason rating

**Verdict: Supported.** Adding preseason rating lowered the mean validation Brier score from **0.0632** to **0.0381** across the four validation seasons. The selected model’s Brier score on the held-out 2025 test season was **0.0425**.

The 2026 Team A and Team C probabilities are forecasts. They cannot be checked against tournament results because 2026 tournament outcomes are not available in the data.

### Recommendation

Do not treat Team C as an easy championship pick based on its 85.7% win rate. In our shared-schedule comparison, the opponent-adjusted model favored Team A, 71.4% to 51.0%. The tournament model also forecast a higher chance for Team A to win at least one bracket game, 80.1% versus 4.6%. The manager should treat Team C as a possible contender, not the clear favorite suggested by its raw win rate.

See [`results/results.md`](results/results.md) for the full tables, validation details, predictions, and figures.

## Reproducing the Results

### Environment

- Python version: 3.14.7
- Install the project packages with `pip install -r requirements.txt`.
- The prepared model data is in `prepared_data/H1/` and `prepared_data/H2/`.

### Notebook order

To rebuild the results from the original data, run:

1. `notebooks/cleaning.ipynb`
2. `notebooks/feature_engineering_H1.ipynb` and `notebooks/feature_engineering_H2.ipynb`
3. `notebooks/H1_Model_Experiments.ipynb`
4. `notebooks/H2_Model_Experiments.ipynb`

### Selected MLflow models

Open each selected run in the course MLflow server and use its logged model artifact to load the model.

| Hypothesis | Selected model | MLflow run ID |
|---|---|---|
| H1 | Opponent-adjusted Bayesian network | **TODO** |
| H2 | Win-rate-plus-rating logistic regression | **TODO** |

### Monitoring

**Proposed review trigger:** Monitor the H1 model’s Brier score as actual game outcomes become available. Review the model if the score over a completed season exceeds **0.25**; this is above the highest H1 walk-forward validation-season score of **0.2218**.

## MLflow Tracking

Model runs and logged artifacts are sent to the course MLflow server at `http://10.0.100.20:5000`. The exported run tables are committed in `results/H1/` and `results/H2/`. The result files and figures are also saved in the repository; see [`results/results.md`](results/results.md).
