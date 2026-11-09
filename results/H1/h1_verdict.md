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

| Model | Team A avg chance | Team C avg chance | Difference (A - C) |
|---|---:|---:|---:|
| Baseline | 0.6129 | 0.5103 | +0.1026 |
| Opponent-adjusted | 0.7138 | 0.5102 | +0.2036 |

Adding opponent strength changed the Team A − Team C difference by +0.1010 (from +0.1026 to +0.2036).

## Verdict

**Not clearly supported by the opponent adjustment.** Team A is ahead under the opponent-adjusted model, as H1 predicts, but Team A was already ahead under the baseline, so adding opponent strength did not reverse the comparison.

*Caveats: results are point estimates with no uncertainty intervals; the 2026 Brier score (accuracy) and the shared-situation comparison answer different questions and should be read separately.*