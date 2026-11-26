# Data requirements

No dataset is distributed. Obtain the original competition data only under its applicable terms: [WSDM Cup 2018 KKBOX Churn Prediction Challenge](https://www.kaggle.com/c/kkbox-churn-prediction-challenge). The original preprocessing scripts are not included, and downloading the raw dataset alone will not make these notebooks runnable.

## Later 78-feature pipeline (01 and 03)

Place compatible pre-engineered tables in `data/parquet/`:

| File | Required content |
|---|---|
| `top_features_train.parquet` | Historical transaction and listening features keyed by `msno`, cutoff 2017-01-31 |
| `top_features_test.parquet` | Same feature schema, cutoff 2017-02-28 (01 only) |
| `members_v3.parquet` | `msno`, `city`, `bd`, `gender`, `registered_via`, `registration_init_time` |
| `train_v2.parquet` | `msno`, `is_churn` |
| `sample_submission_v2.parquet` | Official test user IDs (01 only) |

The later feature matrix merges member attributes and contains 78 model features in the historical run. Notebook 03 additionally expects price, payment, cancellation/renewal, transaction and listening columns, including `last_paid`, `last_price`, `last_discount`, `n_txn`, `autorenew_rate`, `last_ul_days_s`, and `ul_all_active_days`. See notebook code for exact references. The full source feature-generation implementation is not available.

## Earlier prototype (02)

Place `master_train_v2.parquet` and `master_test_v2.parquet` directly under `data/`. Both use the earlier schema, with the model columns in [prototype_feature_columns.json](prototype_feature_columns.json). The training table also requires `is_churn`; both tables use `msno` and payment fields. The saved historical feature list has 155 entries and was recovered from the original notebook. It is not interchangeable with the later 78-feature table.

## Experiment input (04)

Notebook 01 generates `outputs/experiment_risk_input.parquet` locally. It contains risk, activity recency, listed price and outcome, without user IDs. It still contains row-level observations and must not be published.

All raw/processed data and output artifacts should remain local. Only this data description and the historical feature-name list are tracked.
