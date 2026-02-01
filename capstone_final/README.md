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

## Model Selection and Outcomes:

We explored three machine learning models (classification type, mix of unsupervised and supervised) for authentication risk assessment: Isolation Forest, Random Forest, and One-Class SVM. KMeans was leveraged in first part of exercise to identify clusters, it was dropped in final part (Isolation Forest does a better job for anomaly detection problem).  

**Isolation Forest**: Unsupervised anomaly detection, used here to identify initial anomalies and generate the `risk_category` target variable.

**Random Forest Model**: Supervised classification, trained to predict the `risk_category` defined by the Isolation Forest.

**One-Class SVM (OCSVM) Model**: Unsupervised anomaly detection, aiming to learn the boundary of 'normal' behavior and flag deviations.

### Outcome of analysis
A hybrid solution is best suited for this problem, combining the strengths of unsupervised and supervised approaches while addressing their limitations:
1.  **Primary Anomaly Detection model (Unsupervised - OCSVM or Refined Isolation Forest)**:
    *   **Recommendation**: The **One-Class SVM** is better suited for initial, independent anomaly detection due to its unsupervised nature and more realistic performance profile. 
  
2.  **Efficient Classification of Known Patterns (Supervised - Random Forest)**:
    *   **Recommendation**: Once a robust, independently derived, and potentially human-validated set of `risk_category` labels is available, a **Random Forest model** can be highly effective for *rapid and accurate classification of these established anomaly patterns*. This allows for quick automated responses to known threats.
    *   Labels for supervised training must be generated *independently* from the test data.

### Optimal Combination Strategy:
*   **Tiered Approach**: Implement a multi-stage system:
    1.  An **unsupervised model** (OCSVM with optimized parameters) serves as the primary anomaly detector to flag any deviation from normal behavior.
    2.  The alerts from unsupervised layer can then be fed into a **human-in-the-loop system** for validation. This feedback loop is crucial for building a truly labeled dataset over time.
    3.  As sufficient **human-validated anomaly labels** become available, a **supervised model** like Random Forest can be trained on this clean, independent data. This supervised model can then be used for faster, more precise, and automated responses to frequent types of anomalies.

*   **Enhanced Feature Engineering**: Integrate user-specific baseline profiles (`user_id`'s typical time of day for login, location, device, typical IP pattern per User), IP reputation data, and sequential pattern analysis to provide richer context for both unsupervised and supervised models.



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

We divided events almost equally in train and test data sets per risk categorization.

## Model Evaluation: 
We evaluated classification models (listed in earlier section), both supervised and unsupervised. We assessed model performance using **F1-score**, **Precision**, **Recall**, and **Confusion Matrices**, prioritizing these for imbalanced anomaly detection.
No single model was universally optimal. A **hybrid and adaptive approach** is best:

*   **For Novel Anomaly Detection**: **One-Class SVM** (or a refined Isolation Forest) is most optimal, leveraging its unsupervised nature and high anomaly recall to detect new threats.
*   **For Efficient Classification of Known Patterns**: A **Random Forest model** can be highly optimal for *rapidly classifying established anomaly patterns*, but only with robust, independently derived, and potentially human-validated labels (to avoid data leakage).

The optimal solution is a tiered system combining unsupervised methods for broad detection, human feedback for label refinement, and supervised models for efficient classification of confirmed threats, continuously adapting to new information.

## Next Steps: 
We have been constrained with data and computation resources (also time to some extent) in this project. In this exercise, we have looked at subset of login event features and machine learning was based on analysis on a two month events dataset. 
* As per learning from exercise, combining the strengths of One-Class SVM (for novel anomalies) and supervised models like Random Forest (for established patterns) is the best approach for risk assessment for login events data. 
* A better system should include user-specific baseline profiles, external threat intelligence (e.g. IP reputation), and a "human-in-the-loop" feedback mechanism for continuous improvement, dynamic threshold adjustment, and efficient false positive management.

