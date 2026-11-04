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

- `Data/` — The original datasets supplied for the project.
- `notebooks/cleaning.ipynb` — Documents the cleaning steps, checks, and changes in row counts.
- `cleaned_data/` — The datasets produced by the cleaning notebook.
- `cleaning_reports/` — Records the cleaning checks and changes.
- `notebooks/exploration.ipynb` — Explores the headline win-rate comparison and related patterns.
- `figures/` — Contains the domain-input graph, the two hypothesis graphs, and figures from the exploration notebook.
- `hypotheses.md` — States the specific problem, two hypotheses, their predictions, and the planned tests.

## AI-Usage

We used AI to help me learn how to work in Jupyter Notebook, clean the datasets, and create visualizations with Matplotlib. We have reviewed the code and results and made the final decisions about the analysis.

