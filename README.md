# Payment-Prediction-Model
**1. Problem Statement**

The goal of this project is to predict whether a customer will make a payment (EMI) or not using historical loan and behavioral data.

This helps financial institutions:

Identify high-risk customers
Prioritize collection efforts
Improve recovery rates

**2.  Dataset Description**

The dataset contains customer-level loan and payment information over a 6-month period (Feb 2023 – Jul 2023).

Key Features:
Account_number → Unique customer ID
disbursal_date → Loan disbursement date
writeoff_date → Loan write-off date
month_end_date → Reporting month
Pmt_amount → Payment made
dpd_days → Days past due
dpd_bucket → Delinquency category
prod_name → Product type
residence_state → Customer location

**3.  Data Preprocessing**

3.1 Date Conversion

Converted string columns into datetime format:

disbursal_date
writeoff_date
month_end_date

Handled invalid formats using:

errors='coerce'

3.2 Missing Value Treatment

Numerical columns → filled with median
Categorical columns → filled with mode

Median is robust to outliers
Mode preserves categorical distribution

3.3 Outlier Treatment

Applied capping (winsorization):

Lower bound → 1st percentile
Upper bound → 99th percentile

This prevents extreme values from distorting the model.

**4.  Target Variable Creation**

df['paid_flag'] = np.where(df['Pmt_amount'] > 0, 1, 0)
1 → Customer made payment
0 → No payment

Important:
Pmt_amount was removed later to avoid data leakage

**5.  Exploratory Data Analysis (EDA)**

Key Insights Explored:
Distribution of delinquency (dpd_bucket)
Product-wise distribution
State-wise customer distribution
Monthly EMI collection trends
Active vs delinquent behavior

These analyses help understand:

Payment behavior patterns
Risk segmentation

**6.  Feature Engineering & Encoding**

6.1 Dropped Columns

Removed:

Identifiers → Account_number
Date columns → not directly useful without feature extraction
Target leakage → Pmt_amount

6.2 Encoding

Used Label Encoding for categorical variables:

LabelEncoder()


**7.  Train-Test Split**

test_size = 0.2
random_state = 42
80% → Training
20% → Testing

**8.  Feature Scaling**

Applied StandardScaler:

Required for Logistic Regression
Not necessary for Decision Trees

**9.  Model Building**

9.1 Logistic Regression
Baseline model
Interpretable
roc_auc_score = X
Logistic ROC-AUC: 0.8133723589084036

9.2 Decision Tree
Captures non-linear patterns
Handles interactions
max_depth = 5
Decision Tree ROC-AUC: 0.808611236331044

**10.  Model Evaluation**

Metric Used: ROC-AUC Score
Measures model’s ability to distinguish between classes
Better than accuracy for imbalanced data

**11.  Feature Importance**

Extracted from Decision Tree:

feature_importances_

Used to identify:

Key drivers of payment behavior
High-risk indicators

**12.  Key Insights**

Customers with higher DPD (days past due) are less likely to pay
Payment behavior varies across product types
Certain states show higher delinquency trends
Monthly patterns indicate collection fluctuations

**13.  Conclusion**

The model successfully:

Predicts payment probability
Helps identify high-risk customers
Can assist in prioritizing recovery efforts

**14.  Next Steps (Improvements)**

Add time-based features
Days since last payment
Loan age
Include behavioral features
Payment frequency
Historical delinquency
Use advanced models:
XGBoost
Handle class imbalance:
SMOTE
Class weights
