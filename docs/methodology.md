# Methodology and interpretation

## Prediction

The churn prediction pipeline combines XGBoost and LightGBM using five-fold stratified cross-validation within the February cohort. Out-of-fold predictions are used to estimate ensemble weights and fit a logistic calibrator.

A base-rate adjustment targeting a mean predicted churn probability of 3.5% improved the reported private leaderboard log loss from 0.13542 to 0.12611.

Calibration metrics were evaluated on the same out-of-fold predictions used to fit the calibrator, and the target base rate was selected using leaderboard feedback. Independent temporal validation would therefore be necessary to assess prospective performance.

## Initial Intervention Prototype

To move beyond churn prediction, we developed a two-stage intervention framework to identify users who might benefit from discounts or free trials. 

The framework inferred treatment groups from transaction records and combined propensity score matching with a LightGBM-based T-Learner to estimate intervention-related differences in churn probability.

These estimates were combined with assumed customer value and intervention costs through sequential expected net benefit (ENB) rules to generate individualized subsidy recommendations.

The initial 155-feature specification included direct payment information, potentially affecting treatment-effect estimation. The resulting recommendations and ENB estimates were therefore treated as exploratory rather than validated causal effects or realized financial outcomes.

## Revised Diagnostics

To assess the credibility of the initial intervention framework, we conducted a revised analysis using 78 features, excluding direct payment variables from matching and uplift modeling.

Propensity score matching was performed with replacement on logit propensity scores using a 0.2-standard-deviation caliper. Matched outcome contrasts and standardized mean differences (SMDs) were examined to assess covariate balance and treatment-group comparability.

Residual imbalance, limited treatment exposure, and unobserved assignment mechanisms restricted causal interpretation. Maximum absolute SMD remained above 0.1 after matching, and the negative matched contrast for the free-treatment group could not be interpreted as evidence of treatment-induced harm.

We further evaluated S-, T-, and X-Learners using five-fold stratified cross-validation, including all treated users and 100,000 sampled controls for each comparison. Treatment definitions differed slightly between the matching and uplift analyses. Qini metrics were used to assess observational uplift ranking rather than establish causal validity.

Together, these diagnostics motivated a shift from observational subsidy recommendations toward controlled experimentation.

## Proposed Experiment

Given the limitations of observational treatment-effect estimation, we proposed a stratified randomized controlled experiment comparing three intervention strategies: no subsidy, discount, and free trial.

Candidate users were identified using predicted churn risk, recent listening activity, and listed subscription prices. Preliminary sample size estimates were obtained using a two-proportion approximation to assess experimental feasibility.

The proposed evaluation framework considered churn, subscription renewal, customer lifetime value (LTV), and intervention return on investment (ROI).

The candidate estimates were based on in-sample predictions and serve as preliminary planning inputs. Independent eligibility validation and a formal three-arm power analysis would be required before implementation. The proposed experiment was not conducted.
