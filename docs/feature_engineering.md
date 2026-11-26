# Feature Engineering

The feature engineering pipeline integrates historical transaction, listening, and membership data to construct user-level features for churn prediction and intervention analysis.

## Data scale

Approximately 6 million member records, 21.5 million transactions and 392 million daily listening records were available. The team used DuckDB and Parquet partitioning for large log aggregation. The exact upstream scripts are not included.

## Temporal design

| Cohort | Feature cutoff | Prediction target |
|---|---|---|
| Training | 2017-01-31 | February expiration cohort (`train_v2`) |
| Test | 2017-02-28 | March expiration cohort |

Feature windows were defined relative to each cutoff, inspired by the competition's winning solution. This was intended to prevent future-information leakage; the missing upstream scripts prevent a complete audit. Historical observation windows can overlap across cohorts, and the outcome availability timeline should be verified for real deployment.

## Later feature groups

- Transaction timing: days until expiration, days since latest transaction, cancellation and renewal history.
- Payment and plan behavior: price, paid amount, discounts, plan changes and recent transactions.
- Listening behavior: activity counts, total listening time, unique tracks, completion and skip proxies; rolling windows such as 7, 14, 30 and 90 days, prior month and all history; cross-window trends and variability.
- Membership: city, age (invalid values set to missing), gender encoding, registration channel and tenure relative to cutoff.

The later merged prediction matrix had 78 features. The earlier decision prototype instead used 155 original model columns, listed separately in `data/prototype_feature_columns.json`. The diagnostic pipeline removes selected payment-derived covariates because these can directly reveal proxy treatment status. This exclusion alone does not ensure pre-treatment timing or eliminate confounding.

## Source attribution

Dataset: WSDM Cup 2018 KKBOX Churn Prediction Challenge. Temporal feature strategy inspired by Bryan Gregory's winning solution, arXiv:1802.03396. Project-specific implementation and results were documented in the original team report.
