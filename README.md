# Child-Mind-Institute-Problematic-Internet-Use
The goal of this project is to develop a predictive model that analyzes children's physical activity and fitness data to identify early signs of problematic internet use. Identifying these patterns can help trigger interventions to encourage healthier digital habits.

## 📌 **Project Overview**

this project analyzes and predicts problematic internet use (piu) using the child mind institute dataset.
we use sleep, activity, demographic, and survey data to build machine-learning models that classify piu risk levels.

dataset:
https://www.kaggle.com/competitions/child-mind-institute-problematic-internet-use/data

# table of contents
- [project-objectives](#project-objectives)
- [dataset](#dataset)
- [project-structure](#project-structure)
- [installation](#installation)
- [data-preparation](#data-preparation)
- [feature-engineering](#feature-engineering)
- [modeling-approach](#modeling-approach)
- [results](#results)
- [limitations](#limitations)
- [future-work](#future-work)
- [license](#license)
- [references](#references)

---

## project-objectives
- develop predictive models to identify youth at risk for problematic internet use (piu)
- investigate relationships between physical activity / sleep (actigraphy) and sii
- produce reproducible pipelines (eda → fe → models → evaluation)
- publish a clear kaggle notebook + submission ready pipeline

---

## dataset
- competition: child mind institute — problematic internet use  
  https://www.kaggle.com/competitions/child-mind-institute-problematic-internet-use/data

- key files:
  - `train.csv`, `test.csv`, `sample_submission.csv`
  - `series_train.parquet/*` (actigraphy time-series)
  - `series_test.parquet/*` (actigraphy time-series)

---

## project-structure

---
## data-preparation

read csv & parquet (polars/pandas)

per-id aggregation of time-series (mean/std/skew/kurtosis/enmo)

non-wear detection & masking

imputation: knn / simple mean/median for numeric; mode for categoricals

save processed sets to data/processed/

---

## feature-engineering

time-series aggregates (per-day / per-week): mean, std, max, min, pctiles

derived features: bmiage, internet-hoursage, enmo-norm, sleep-efficiency

autoencoder embedding for time-series (optional)

feature selection via correlation thresholding & tree-based importance

---

## modeling-approach

target: sii (ordinal 0..3). approaches:

regression + threshold tuning (optimize QWK)

ordinal classification (optional)

models:

catboost, xgboost, lightgbm

voting regressor ensemble

optional: MLP / CNN / LSTM on raw actigraphy

validation:

stratified k-fold (n=5) based on sii

track oof preds and use scipy.optimize for threshold tuning

---


## results

| model           | accuracy | precision | recall | f1-score | cohen-kappa |
|-----------------|----------|-----------|--------|----------|-------------|
| catboost        | 0.3289   | 0.34      | 0.33   | 0.325    | 0.10        |
| lightgbm        | 0.4794   | 0.48      | 0.48   | 0.47     | 0.21        |
| xgboost         | 0.4066   | 0.41      | 0.40   | 0.40     | 0.16        |
| **voting-regressor** | **0.5474** | **0.5946** | **0.5474** | **0.5387** | **0.3465** |

---

## limitations

missing data and imputation bias

cross-sectional data: cannot infer causality

limited demographic diversity may limit generalizability

sensor noise and variations in wear-time may affect actigraphy features

---

## future-work

ordinal regression specialized models (proportional odds)

end-to-end deep learning on raw actigraphy (1D-CNN / transformer)

longitudinal study to probe causality

explainability (shap for per-subject explanations)

---

## license

this project is released under the mit license. see LICENSE for details.

## references

child mind institute — problematic internet use competition (kaggle):
https://www.kaggle.com/competitions/child-mind-institute-problematic-internet-use/data

literature on actigraphy and adolescent behavior (see REFERENCES.md in repo)

