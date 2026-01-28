# CapStone Project : Module 20 - Initial Report and EDA
Authentication Risk Assessment

### Project Title
Authentication Risk Assessment

#### Link of notebook file

https://github.com/rakeshagf/authn_risk_assessment/blob/main/authn_risk_assessment_part1.ipynb 

#### Executive summary
Part 1: Exploratory Data Analysis (EDA) and Basic Modeling:

The primary objective of Exploratory Data Analysis (EDA) is to transform raw login event data into a clean, structured, and feature-engineered format suitable for developing machine learning models. These models will be used to predict whether a login attempt is normal or anomalous. The ultimate goal is to enhance authentication risk assessment by leveraging insights that enforces decisions on whether to allow a user to log in with regular authentication, require additional strong authentication, or block/alert the authentication attempt. 
EDA lays the groundwork for building robust models that can accurately assess authentication risk and support a secure and efficient user experience.

Basic Modeling

K Means and Isolation Forest, Random Forest -> Results

Part 2 (final): Tuning + Random Forest and One Class SVM

#### Research Question
predicting user login risk (low/high) for a new authentication attempt

#### Rationale
These models will be used to predict whether a login attempt is normal or anomalous. The ultimate goal is to enhance authentication risk assessment by providing insights to decide on whether to allow a user to log in with regular authentication, require additional strong authentication, or block/alert the authentication attempt.

#### Data Sources
I have access to customer login data from Customer Identity and Access Management (IAM) event logs.  I have removed unnecessary columns, anonymized (including synthetic values) data for select fields. CSV file containing login events is named 'authn_event_logs.csv' and added to data folder.

#### Methodology
I plan to leverage mix of unsupervised and supervised learning techniques for this exercise: 

Initial 2 techniques are intended to develop a new feature to label risk assessment outcome which will be used for Random Forest model. 
K-Means Clustering (Unsupervised)
Isolation Forest (Supervised)

These two techniques will be used to predict risk assesment outcomes for new/unseen login events:
Random Forest (Supervised)
One-Class SVM (Unsupervised)


#### Results
# Summary of Key Findings

This part of project involved a comprehensive Exploratory Data Analysis (EDA) and initial unsupervised modeling to identify patterns and potential anomalies in login event data. The key findings are as follows:

### 1. Data Cleaning and Feature Engineering
*   **Data Preparation**: Initial raw login event data was cleaned by handling missing values, standardizing entries (e.g., `request_ip_state`), and removing irrelevant columns. Specifically, 2 records with missing `client_country` were dropped, and `request_ip_state` missing values were imputed.
*   **Feature Creation**: Key new features were engineered from existing data to enhance analytical depth:
    *   `time_of_day`: Hour of login for temporal pattern analysis.
    *   `country_category`: Binary classification of login origin as 'United States' (1) or 'International' (0).
    *   `state_category`: Granular geographical context into 'Top 20 US State' (5), 'Remaining US State' (4), 'Unknown US State' (3), 'International State' (2), and 'Unknown International State' (1).
    *   `device_category`: Binary representation of device type ('Mobile/Tablet' as 0, 'Computer' as 1).

### 2. K-Means Clustering for Behavioral Segmentation
*   **Cluster Discovery**: K-Means clustering (2 clusters) effectively segmented login events into two distinct behavioral patterns:
    *   **Cluster 0 (Larger, ~62% of events)**: Characterized exclusively by 'Computer' logins, with a mean `time_of_day` around 14.86 (2:52 PM), predominantly from the United States and Top 20 US States. This represents the high-volume, regular 'Computer' login behavior.
    *   **Cluster 1 (Smaller, ~38% of events)**: Characterized exclusively by 'Mobile/Tablet' logins, with a mean `time_of_day` around 13.64 (1:38 PM), also predominantly from the United States and Top 20 US States. This cluster highlights regular 'Mobile/Tablet' login activities.
*   **Key Differentiator**: The most significant factor distinguishing these two K-Means clusters was the `device_category`.

### 3. Isolation Forest for Anomaly Detection
*   **Model Application**: Isolation Forest was applied to the `time_of_day`, `country_category`, `state_category`, and `device_category` features to detect anomalous login events, using a `contamination` rate of 0.05. The model identified 4,888 outliers.
*   **Outlier Characteristics (Comparison to Inliers)**: Analysis comparing outliers to inliers revealed significant differentiators:
    *   **Geographical Context**: Outliers showed a significantly higher proportion from 'International' locations (around 14.14%) and 'Remaining US States' (around 66.86%), while inliers were almost exclusively from 'United States' and 'Top 20 US State'.
    *   **Time of Day**: Outliers had a slightly earlier mean login time (~1:36 PM) compared to inliers (~2:26 PM), suggesting deviations from the most frequent login hours.
    *   **Device Type**: The device distribution (Computer vs. Mobile/Tablet) was relatively similar between outliers and inliers, indicating it is not a primary distinguishing factor for anomalous behavior.

### 4. Random Forest Model for Anomaly Prediction
*   A Random Forest classifier was trained using the features (`time_of_day`, `country_category`, `state_category`, `device_category`) to predict the `anomaly_prediction` labels generated by the Isolation Forest model.
*   The Random Forest model achieved **perfect accuracy (1.00)**, with 100% precision, recall, and F1-score for both inlier and outlier classes. This demonstrates that the Random Forest successfully learned the underlying patterns identified by the Isolation Forest.
*   **Feature Importance**: The `state_category` was found to be by far the most important feature, followed by `time_of_day` and `country_category`, while `device_category` had the least importance.

### 5. Conclusion
The EDA successfully transformed raw login event data into a structured format suitable for anomaly detection. Isolation Forest identified that anomalous login events are predominantly characterized by their **international geographical origin or less common US states** and occurrence at **times deviating from peak login activity**, rather than specific device types. These geographical and temporal patterns are robust indicators of potential anomalies for authentication risk assessment, and the Random Forest model effectively captured these relationships.

#### Next steps
In final part of capstone project (seperate notebook file), we will revisit Isolation Forest model. In addition to updating Isolation Forest model, we will tune Random Forest parameters and implement One Class SVM. 
