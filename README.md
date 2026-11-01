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

4. **How have Team C’s preseason ratings changed over time, and how did its 2026 results compare with preseason expectations?**  
   **Goal:** See whether Team C appears to have been improving and whether its 2026 performance was unexpected.

5. **Across past seasons, how did teams with high regular-season win rates perform in the national tournament?**  
   **Goal:** See whether a high win rate has usually been a good sign of tournament success.

## Repository Guide
- `notebooks/cleaning.ipynb` — Loads the data we use, documents cleaning steps, and reports row counts.
- `notebooks/exploration.ipynb` — Explores Team A and Team C’s win rates and related patterns.
- `figures/domain_inputs_graph.png` — Shows the relationships described by the domain experts and the claim being investigated.
- `hypotheses.md` — States our narrowed problem, two hypotheses, predictions, and planned tests.
- `Data/` — Contains the supplied project datasets.

## Reproducing the Work


