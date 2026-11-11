# RMDS Hackathon — Group G

## Project overview

This project predicts whether respondents will accept an H1N1 vaccination and explores how vaccination offers could affect a town's expected expenditure. The assignment considers both vaccination-related costs and hospitalisation costs.

The supplied dataset includes demographic, behavioural, health and H1N1 attitude variables. The target is `h1n1_vaccine` (0 = no, 1 = yes).

## Data

- Training data: **4,756 respondents**, with **29 predictor features** and one binary target.
- Test data: **4,749 respondents**, with **29 predictor features**.
- Training target distribution: 2,882 unvaccinated (0), 1,874 vaccinated (1).
- Stratified validation split: 3,804 training rows and 952 validation rows.

The data files are not included in this README. Use the datasets provided for the hackathon.

## Workflow

1. Explore missing values, vaccination rates and relationships with health and demographic features.
2. Build a preprocessing pipeline for 19 numeric-coded and 10 categorical features, including missing-value handling.
3. Train Logistic Regression, Random Forest and CatBoost models.
4. Tune Logistic Regression and evaluate a soft-voting ensemble.
5. Compare models using accuracy, precision, recall, F1 and ROC-AUC on the validation split.
6. Explore a **scenario-based** hospitalisation-cost model and sensitivity analysis.
7. Generate and validate five binary prediction CSVs.

## Validation results

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | 0.7605 | 0.7290 | 0.6240 | 0.6724 | 0.8164 |
| Random Forest | 0.7584 | 0.7377 | 0.6000 | 0.6618 | 0.8022 |
| CatBoost | 0.7563 | 0.7097 | 0.6453 | 0.6760 | 0.8061 |
| Soft-voting Ensemble | 0.7637 | 0.7389 | 0.6187 | 0.6734 | 0.8151 |
| Tuned Logistic Regression | 0.7574 | 0.7250 | 0.6187 | 0.6676 | 0.8145 |

The original Logistic Regression had the highest reported ROC-AUC among these approaches. A separate threshold experiment found that 0.60 had the lowest estimated validation cost among the thresholds tested.

## Illustrative economic analysis

Assignment costs:

- H1N1 vaccine or compensation when unavailable: **$500** per person.
- H1N1 hospitalisation: **$25,000** per hospitalisation.

The dataset does **not** provide hospitalisation outcomes. For a demonstration, hospitalisation risk was assigned using hypothetical age-specific baselines (1%, 1.5%, 2%, 3.5%, 6%) and a 50% increase relative to baseline per chronic medical condition. Vaccine effectiveness was assumed to be 80% in the main scenario.

Under those assumptions, for the 4,749 test respondents:

| Strategy | Estimated total cost |
|---|---:|
| No vaccination offers | $4,564,187.50 |
| Offer vaccination to everyone | $5,383,254.21 |
| Targeted vaccination offers | $4,125,469.66 |

The targeted scenario offered vaccination to 1,001 respondents and showed **$438,717.84 (9.61%)** in expected savings versus no offers. Sensitivity analysis across hypothetical vaccine effectiveness levels of 40%, 60%, 80% and 100% gave estimated savings of $69,070.30, $226,980.13, $438,717.84 and $690,145.33, respectively.

**Limitations:** These are illustrative, assumption-dependent estimates—not measured H1N1 hospitalisation probabilities, clinically validated vaccine effectiveness, or confirmed savings. The cost comparison treats a predicted acceptance as a decision to offer vaccination; this is a proxy policy evaluation and may differ from the hackathon's official scoring rules. The 0.60 threshold was selected on the same validation split used for the cost comparison, so its apparent advantage may not generalise.

## Submission files

All files contain exactly two columns, `respondent_id` and `h1n1_vaccine`, with **4,749 rows**, unique IDs, no missing values and binary predictions.

| File | Method | Predicted vaccinated (1) |
|---|---|---:|
| `challenge_submission_group_G_order_1.csv` | Logistic Regression, threshold 0.60 | 1,301 |
| `challenge_submission_group_G_order_2.csv` | Soft-voting Ensemble | 1,626 |
| `challenge_submission_group_G_order_3.csv` | Logistic Regression, default threshold | 1,647 |
| `challenge_submission_group_G_order_4.csv` | Random Forest | 1,522 |
| `challenge_submission_group_G_order_5.csv` | CatBoost | 1,693 |

The ordering follows a **provisional, scenario-based validation-cost ranking**, not a verified hidden-test or official evaluation ranking.

## Reproducing the work

1. Open the Group G Google Colab notebook.
2. Upload the supplied training and test datasets when prompted.
3. Run notebook cells in order: exploration, preprocessing, modelling, evaluation, cost scenarios and export.
4. Set the group identifier to `G` in the submission export code.
5. Check that each exported CSV has the required two columns and 4,749 rows before uploading the five CSVs individually to Canvas.

### Notebook and code links

- Google Colab: **[Add your shareable notebook link]**
- GitHub repository (if used): **[Add your repository link]**

## Responsible interpretation

The prediction target (`h1n1_vaccine`) represents **vaccination acceptance**, not the separate decision of whether to offer a vaccine. Do not submit the `optimal_offer` column in place of vaccination-acceptance predictions. Real-world health policy would require independently validated risk estimates and a broader assessment beyond this exercise's simplified financial objective.
