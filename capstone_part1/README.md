# Capstone Project — Module 20: Initial Report and EDA

#### Project Title
Authentication Risk Assessment

Author: Rakesh Meena

#### Notebook (Part 1):
https://github.com/rakeshagf/authn_risk_assessment/blob/main/capstone_part1/authn_risk_assessment_part1.ipynb

#### Data sources
- The dataset contains anonymized customer login event logs from a Customer Identity and Access Management (IAM) system.
- The CSV used in this analysis is data/authn_event_logs.csv (anonymized and pre-processed).

#### Executive summary
This repository contains the exploratory data analysis (EDA) and initial modeling for an authentication risk assessment capstone project. The goal is to detect anomalous (high-risk) login attempts and build models that can help flag risky authentications in production.

Project structure and scope
- Part 1 (this notebook): EDA, feature engineering, exploratory unsupervised models (K-Means, Isolation Forest), and an initial supervised model (Random Forest) trained to predict the anomaly labels produced by Isolation Forest.
- Part 2 (final): Model tuning, additional supervised/unsupervised approaches (e.g., One-Class SVM), and evaluation on holdout data.

#### Research question
- Predict whether a new user login attempt is low-risk (normal) or high-risk (anomalous).

#### Rationale
AI/ML models will be used to predict whether a login attempt is normal or anomalous. The ultimate goal is to enhance authentication risk assessment by providing insights to decide on whether to allow a user to log in with regular authentication, require additional strong authentication, or block/alert the authentication attempt.


#### Methodology
1. EDA and feature engineering to convert raw logs into model-ready features:
   - `time_of_day`: hour of login (0–23)
   - `country_category`: 1 = United States, 0 = International
   - `state_category`: categorical encoding for US/state granularity (Top-20, Remaining, Unknown, International, Unknown International)
   - `device_category`: 1 = Computer, 0 = Mobile/Tablet

2. Unsupervised techniques to identify anomalous behavior:
   - K-Means for behavioral segmentation
   - Isolation Forest for anomaly detection (used to generate labels for supervised learning)

3. Supervised technique:
   - Random Forest trained on engineered features to predict anomaly labels from Isolation Forest. (Part 2 will include hyperparameter tuning and additional models.)

#### Results
# Summary of Key Findings
- K-Means (k=2) separated events primarily by device type (Computer vs Mobile/Tablet) with different typical login hours.
- Isolation Forest identified a small set of outliers that were more likely to originate from international locations and less-common US states, and that had a slightly different time-of-day distribution.
- A Random Forest trained to predict Isolation Forest labels achieved very high performance on the dataset used in Part 1; feature importance indicated `state_category` and `time_of_day` were the most informative features.

Notes and caveats
- The dataset has been anonymized and/or synthetically altered for privacy—results are illustrative.
- The perfect performance of the Random Forest in Part 1 likely indicates label leakage or an easy separability introduced by the pipeline; Part 2 will investigate proper validation and tuning.

### Next steps (Part 2)
- Re-evaluate Isolation Forest settings and contamination parameter.
- Tune Random Forest (cross-validation, hyperparameter search) and add robust evaluation on holdout data.
- Experiment with One-Class SVM and additional anomaly-detection techniques.
- Add documentation and a final report summarizing insights and potential deployment considerations.

### License & author
- Author: Rakesh Meena
