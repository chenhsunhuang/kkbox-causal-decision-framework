# Methodology and interpretation notes

## Prediction

The original later pipeline uses five-fold stratified CV within the February cohort. Ensemble weights and the logistic calibrator are fitted on OOF predictions, and the same predictions are used to report calibration log loss. This is not a nested calibration evaluation. The original XGBoost prediction does not explicitly restrict the prediction iteration range to the best iteration; final full-data refits use the last fold's best iteration, rather than aggregating fold-specific iteration counts. These are retained historical choices, not recommendations. The 3.5% test mean was selected using leaderboard feedback, not independent prior knowledge. An untouched temporal cohort is needed to assess prospective calibration and performance.

## Initial intervention prototype

The early two-stage notebook infers treatment from the relationship between listed and paid price. Stage 1 compares positive-payment discounts to full payment; stage 2 compares a narrower zero-payment group (positive listed price and no cancellation) to full payment. It fits a propensity model and nearest-neighbor matches, then separate LightGBM outcome models. Its probability-difference scores are combined with assumed customer value and treatment cost in sequential ENB rules. The 155-feature outcome schema includes direct payment information and may contain treatment-related variables; the propensity cutoff is a heuristic, not an identification guarantee. The listed price is not necessarily monthly value. Thus the historical assignment counts and model-estimated ENB are exploratory outputs, not validated treatment effects or realized revenue.

## Revised diagnostics

The later 78-feature pipeline uses a slightly different proxy treatment definition and excludes direct payment variables from matching and uplift covariates. Exact treatment assignment time and targeting logic are not observed. Matching with replacement on logit propensity uses a 0.2-SD caliper. The code's historical `ATE` variable is an ATT-like matched contrast; naive independent-proportion intervals ignore matched dependence and propensity estimation. Maximum absolute SMD remains above 0.1 after matching. Sparse treated rows and possible residual confounding limit interpretation. The negative free-group contrast does not demonstrate either harm or a specific assignment policy.

S/T/X-Learners are evaluated with five stratified folds on all treated users and 100,000 sampled controls for each comparison. Qini is an observational ranking diagnostic, not a causal validation or directly comparable industry benchmark. Differences in treatment definitions across the matching and uplift routines are preserved from the original code.

## Proposed experiment

The historical full-pipeline candidate filter uses in-sample fitted risk, recent listening, and listed price. Its candidate count and baseline rate are illustrative planning inputs. An independent cohort or cross-fitted risk estimates should be used before deployment. The original per-arm calculation is an equal-size two-proportion approximation, not a full three-arm multiplicity-adjusted power analysis. Define eligibility, treatment costs, treatment timing, primary endpoints, stratification and analysis plan prospectively. The proposed experiment was not run.

## Portfolio edition changes

- Split the later full notebook into prediction and experiment-design sections; use the standalone decision notebook for the revised diagnostics and SMD output.
- Retain core early-prototype code; replace the missing pickle dependency with its saved 155-feature list, remove redundant retraining and optional historical comparison/export cells, and keep only aggregate output logic.
- Translate headings and commentary, remove original cached outputs and user-level export displays, and replace absolute paths with local repository-relative paths.
- Preserve historical modeling assumptions rather than silently revising results. Add the matched-row count as a diagnostic and save a local intermediate for experiment design.
- Figures are regenerated from recorded aggregate results; no original model was retrained or original data redistributed. The final-report score 0.12611 is used consistently; the pipeline notes' 0.12556 is not used because it is not supported by the supplied final notebook and report.
