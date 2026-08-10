**Overview**

This repository contains my completed submission for the WGU course **D683 – Implementing and Testing AI Solutions**.

The project delivers an end-to-end supervised machine learning pipeline for the DQN1 case study: **Urban Air Quality and Health Risk Prediction**. The pipeline ingests raw environmental and health data, engineers features, tunes a Gradient Boosting model through randomized hyperparameter search, evaluates performance on a held-out test set, and automatically produces a full set of reproducible artifacts for each prediction target.

The emphasis of this course is not only on building a model that works, but on implementing it as a maintainable, testable, and auditable system.

**Scenario**

A public health organization needs to understand and forecast the relationship between urban air pollution and population health outcomes. Using a tabular dataset of environmental readings and health indicators, the objective is to build predictive models for multiple targets, including:

-PM2.5 particulate concentration

-Health risk score

-Severity score

Because environmental data is time-ordered, the pipeline must avoid data leakage between past and future observations when validating model performance.

**Project Objectives**

-Build a reusable, parameterized machine learning pipeline rather than a one-off script

-Implement robust preprocessing that handles missing values and mixed data types

-Support time-aware validation to prevent look-ahead bias

-Tune model hyperparameters systematically rather than manually

-Evaluate models against multiple error metrics

-Automatically generate artifacts that make results reproducible and reviewable

-Support multiple prediction targets from a single command

**Technical Implementation**

**Data Ingestion**
Reads Excel source data with pandas and openpyxl. Includes a column discovery mode that inspects the dataset and heuristically suggests plausible numeric prediction targets.

**Feature Engineering**
When a time column is supplied, the pipeline derives calendar features (year, month, day, day of week, hour) from epoch timestamps, with automatic detection of second-based versus millisecond-based epochs.

**Preprocessing Pipeline**
A scikit-learn ColumnTransformer applies median imputation to numeric features and most-frequent imputation plus one-hot encoding to categorical features. Unknown categories at prediction time are handled gracefully rather than raising errors.

**Model and Tuning**
A GradientBoostingRegressor is wrapped in a scikit-learn Pipeline and tuned using RandomizedSearchCV across estimator count, learning rate, tree depth, subsample ratio, minimum leaf size, and feature sampling strategy. The search optimizes negative root mean squared error.

**Validation Strategy**
When a date column is provided, the pipeline uses a chronological train/test split combined with TimeSeriesSplit cross-validation. Without a date column, it falls back to a randomized split with K-Fold cross-validation.

**Evaluation and Testing**
Each model is scored on RMSE, MAE, and MAPE against a held-out test set. Feature importances are extracted from the fitted estimator and mapped back to human-readable feature names, including one-hot expanded categories.

**Artifact Generation**
For every target, the pipeline writes a dedicated artifacts directory containing metrics in both JSON and CSV form, a predictions file pairing actual against predicted values, a ranked feature importance table, a predicted-versus-actual scatter plot, a top-25 feature importance chart, and a per-target summary README.

**Skills Demonstrated**

-End-to-end machine learning pipeline design

-Time-aware validation and data leakage prevention

-Automated hyperparameter optimization

-Robust preprocessing for mixed-type tabular data

-Regression model evaluation and error analysis

-Feature importance extraction and interpretation

-Reproducible experiment tracking and artifact management

-Command-line tool design with argument parsing

-Headless plotting and automated visualization

**How to Run**

Install dependencies:

```
pip install -r requirements.txt
```

Inspect available columns and suggested targets:

```
python dqn1_model.py --data "DQN1 Dataset.xlsx" --list-columns
```

Train with a time-aware split:

```
python dqn1_model.py --data "DQN1 Dataset.xlsx" --target "PM2.5" --datecol sunriseEpoch
```

Train multiple targets in a single run:

```
python dqn1_model.py --data "DQN1 Dataset.xlsx" --target "PM2.5,health_risk_score" --datecol sunriseEpoch
```

**Repository Contents**

-`dqn1_model.py` – Full training, tuning, evaluation, and artifact generation pipeline

-`requirements.txt` – Python dependencies

-`artifacts/pm2.5/` – Metrics, predictions, feature importances, and plots for PM2.5 prediction

-`artifacts/healthRiskScore/` – Metrics, predictions, feature importances, and plots for health risk prediction

-`artifacts/severityScore/` – Metrics, predictions, feature importances, and plots for severity prediction
