# Capstone Project (Module 24): Final Report

## Project Title
Authentication Risk Assessment

Author: Rakesh Meena

#### Notebook Link:


## Problem Statement:

**Goal**: The primary goal is to develop an authentication risk assessment system for a banking application(s) that can accurately classify customer login attempts as 'normal' or 'anomalous' (abnormal).

**Challenge (without ML based solution)**: Traditional authentication methods often rely on static authentication policies, which doesn't offer a great combination of user experience and security. The challenge includes managing the trade-off between false positives (legitimate users inconvenienced with multiple factors) and false negatives (missed security threats).

**Proposed Solution**: Leverage advanced machine learning models, including unsupervised techniques like Isolation Forest and One-Class SVM for anomaly detection, and supervised methods like Random Forest for classification. These models can analyze customer login event data (e.g., time of day, geographical location, device type) to identify patterns indicative of suspicious activity. 

**Potential Benefits**: This machine learning solution aims to enhance security and improving user experience for normal login events.
Automation of login anomaly detection also reduces the burden on security operations teams, allowing them to focus on investigating high-priority threats rather than sifting through vast amounts of data manually. 

## Model Outcomes or Predictions:
4.	Identify the type of learning (classification or regression) and specify the expected output of your selected model. Determine whether supervised or unsupervised learning algorithms will be used.

## Data Acquisition:
- The dataset contains anonymized customer login event logs from a Customer Identity and Access Management (IAM) system. We have already completed Exploratory Data Analysis (EDA) in first part of exercise. 
- The CSV used in this analysis is data/login_events_final.csv (anonymized and pre-processed); this file was generated in part 1 as a result of EDA and feature engineering.

## Data Preprocessing/Preparation: 

*   **Data Preparation**: Initial raw login event data was cleaned by handling missing values, standardizing entries, and removing irrelevant columns. Specifically, few records with missing `client_country` were dropped, and `request_ip_state` missing values were imputed.
*   **Feature Creation**: Key new features were engineered from existing data to enhance analytical depth:
    *   `time_of_day`: Hour of login for temporal pattern analysis.
    *   `country_category`: Binary classification of login origin as 'United States' (1) or 'International' (0).
    *   `state_category`: Granular geographical context into 'Top 15 US State' (5), 'Remaining US State' (4), 'Unknown US State' (3), 'International State' (2), and 'Unknown International State' (1).
    *   `device_category`: Binary representation of device type ('Mobile/Tablet' as 0, 'Computer' as 1).
Instead of using one hot encoding, we created new category features for country, state and device to define numeric values for categorization. It helped in keeping number of features low without loss of information. 
**Data split into training and test sets**
The data was split into training and test sets using the `train_test_split` function from `sklearn.model_selection`. 

1.  **Features (X)**: The columns `time_of_day`, `state_category`, and `device_category` were selected as input features.
2.  **Target (y)**: The `risk_category` column, derived from the Isolation Forest predictions, was used as the target variable.
3.  **Test Size**: 30% of the data (`test_size=0.3`) was allocated for the test set, and the remaining 70% for the training set.
We divided events almost equally in train and test data sets as per risk categorization.


## Modeling: 
For this deliverable, please document your selection of machine learning algorithms that you selected for your problem statement from the first deliverable.

## Model Evaluation: 
Share your model evaluation here. What types of models did you consider for your problem (classification, regression, unsupervised)?  Articulate the evaluation metrics you used and how you determined which model was most optimal for your problem.

## Next Steps: 
