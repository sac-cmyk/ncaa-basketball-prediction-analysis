# H1 Verdict

## Validation (Brier score, lower is better)

| Model | 2021 Brier | 2022 Brier | 2023 Brier | 2024 Brier | 2025 Brier | Mean | Spread (SD) | Train-validation gap |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Baseline | 0.2471 | 0.2263 | 0.2227 | 0.2241 | 0.2269 | 0.2295 | 0.0100 | -0.0006 |
| Opponent-adjusted | 0.2218 | 0.1962 | 0.1935 | 0.1973 | 0.1976 | 0.2013 | 0.0116 | -0.0020 |

**Stability:** Opponent-adjusted had the lower Brier in 5 of 5 held-out seasons. Spread across seasons: Baseline 0.0100, Opponent-adjusted 0.0116.

## Model selected for the final test

**Opponent-adjusted** was selected because it had the lower mean validation Brier (0.2013 vs 0.2295 for Baseline).
Its 2026 test Brier score (fit on 2015–2019 and 2021–2025, scored once) is **0.2003**.

## Team A vs. Team C on shared 2026 situations

The comparison uses `h1_shared_2026_scenarios.csv` (58 situations, one row per team in each, 116 rows). In every situation Team A and Team C have a matching opponent rating group and game location. Team A = team id 1105 and Team C = team id 1120. No game outcomes are used.

| Model | Team A avg chance | Team C avg chance | Difference (A - C) |
|---|---:|---:|---:|
| Baseline | 61.3% | 51.0% | +10.3 pts |
| Opponent-adjusted | 71.4% | 51.0% | +20.4 pts |

Adding opponent strength changed the Team A − Team C difference by +10.1 percentage points (from +10.3 to +20.4).

## Verdict

**H1's prediction was supported.** The opponent-adjusted model estimated Team A at 71.4% and Team C at 51.0% on the shared situations (matching opponent rating group and location), so Team A has the higher estimated chance of winning.

**Limitation:** the baseline model already favored Team A (61.3% vs. 51.0%), so Team A was ahead before opponent strength was added. Including opponent rating group changed the gap from +10.3 to +20.4 percentage points (Team A moved +10.1 points; Team C moved +0.0). We therefore cannot say that opponent strength alone caused the difference between the teams.

*Caveats: results are point estimates with no uncertainty intervals; the 2026 Brier score (accuracy) and the shared-situation comparison answer different questions and should be read separately.*