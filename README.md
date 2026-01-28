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
What did your research find?

#### Next steps
In final part of capstone project (seperate notebook file), we will revisit Isolation Forest model. In addition to updating Isolation Forest model, we will tune Random Forest parameters and implement One Class SVM. 
