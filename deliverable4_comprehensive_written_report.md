# Advanced Data Mining Project
## Deliverable 4 Comprehensive Written Report

### Title Page
- Project Title: Customer Behavior Mining in E-Commerce
- Course: Advanced Data Mining
- Deliverable: 4 - Final Insights, Recommendations, and Presentation
- Student: Sandra Luwedde
- Date: March 2026

## 1. Introduction
This project analyzes e-commerce customer behavior data to extract actionable insights through a complete data mining lifecycle. The work is structured across four deliverables: data preparation and exploration, regression modeling, classification/clustering/pattern mining, and final synthesis with recommendations and ethical reflection.

## 2. Dataset and Motivation
- Dataset: ecommerce_user_dataset_cleaned.csv
- Records: 1,000
- Attributes: 8
- Domain: customer behavior and segmentation

The dataset was selected because it satisfies project size and feature requirements and supports multiple mining tasks:
- Regression on continuous outcomes (Monetary_Value)
- Classification of Customer_Segment
- Clustering for latent group discovery
- Association rule mining for behavior pattern extraction

## 3. Data Preparation and EDA
### 3.1 Preparation Steps
- Loaded and validated schema and data types
- Checked missing values and duplicate records
- Verified bounded feature constraints (for example Engagement_Score in [0,1])
- Applied reusable cleaning logic and outlier capping approach in earlier stages

### 3.2 EDA Highlights
- Customer segment imbalance exists (majority and minority segment split)
- Weak linear correlations among some predictors suggest benefits from richer feature engineering
- Behavioral distributions and segment-level summaries revealed non-trivial but subtle differences

### 3.3 Feature Engineering
Engineered features used across modeling stages:
- purchase_freq_interaction = Purchase_History * Transaction_Frequency
- engagement_time_interaction = Engagement_Score * Time_on_Site
- browsing_per_visit = Browsing_Behavior / (Transaction_Frequency + 1)
- purchase_per_visit = Purchase_History / (Transaction_Frequency + 1)

These features improved signal capture for both predictive and descriptive modeling tasks.

## 4. Modeling and Evaluation
### 4.1 Regression Modeling
Models:
- Linear Regression
- Ridge Regression
- Lasso Regression

Evaluation metrics:
- R-squared
- Mean Squared Error (MSE)
- Root Mean Squared Error (RMSE)
- 5-fold cross-validation (CV R2 and CV RMSE)

Summary:
- Regularized and baseline linear models were compared under identical preprocessing.
- Cross-validation was used to prioritize generalization rather than one-split performance.

### 4.2 Classification Modeling
Models:
- Decision Tree Classifier
- k-Nearest Neighbors (k-NN)

Hyperparameter tuning:
- GridSearchCV on k-NN with n_neighbors, weights, and metric

Evaluation metrics:
- Confusion matrix
- ROC curve
- Accuracy
- F1 score

Summary:
- F1 was prioritized due to class imbalance.
- Tuned models provided more reliable selection than untuned baselines.

### 4.3 Clustering
Model:
- K-Means (k=3)

Evaluation/interpretation:
- PCA-based 2D visualization for cluster separation
- Cluster profile table using mean feature values

Summary:
- Clustering identified interpretable customer groups with different engagement and purchase patterns.

### 4.4 Association Rule Mining
Technique:
- Apriori frequent itemset mining
- Association rules based on confidence and lift

Preparation:
- Discretized continuous features into quantile bins (low/mid/high)
- Constructed transaction-like encoded records

Summary:
- High-lift rules identified co-occurring customer behavior states.
- Rules support recommendation and campaign design decisions.

## 5. Key Integrated Insights
- Data quality and preprocessing enabled stable downstream modeling.
- Feature engineering improved representational power across tasks.
- Classification performance should be interpreted with imbalance-aware metrics.
- Clustering and association rules complemented supervised learning by revealing latent patterns.
- Combining predictive and descriptive analytics gave stronger business value than any single method.

## 6. Practical Recommendations
- Use segment-aware campaigns based on classification predictions and cluster profiles.
- Prioritize high-engagement, moderate-value groups for upsell/retention experiments.
- Deploy recommendation triggers derived from high-lift behavior rules.
- Establish periodic retraining and monitoring to handle behavior drift.
- Track business KPIs (conversion uplift, retention, average order value) alongside model metrics.

## 7. Ethical Considerations
### 7.1 Data Privacy
- Excluded direct identifier fields from modeling inputs.
- Recommend minimum-necessary access policies and secure handling of customer data.

### 7.2 Fairness and Bias
- Segment imbalance can lead to biased classification behavior.
- Addressed by emphasizing F1, confusion matrix, and ROC rather than accuracy alone.
- Recommend fairness audits per segment before deployment.

### 7.3 Responsible Use
- Predictions should assist decision-making, not enforce rigid automated actions.
- Maintain human review for high-impact business decisions.
- Avoid manipulative targeting strategies for vulnerable users.

## 8. Challenges and Mitigations
- Challenge: weak simple linear signal in raw features.
  - Mitigation: interaction and ratio-based feature engineering.
- Challenge: potential overfitting and unstable model selection.
  - Mitigation: cross-validation and hyperparameter tuning.
- Challenge: translating numeric behavior into rules.
  - Mitigation: discretization and transaction encoding for Apriori.

## 9. References
- Pedregosa et al. (2011). Scikit-learn: Machine Learning in Python.
- Agrawal et al. (1994). Fast Algorithms for Mining Association Rules.
- Han, Kamber, and Pei. Data Mining: Concepts and Techniques.

## 10. Presentation Notes (5-7 Minutes)
1. Problem and dataset motivation (45 sec)
2. Data preparation and EDA highlights (60 sec)
3. Regression and classification outcomes (75 sec)
4. Clustering and association rules (75 sec)
5. Ethical considerations and recommendations (60 sec)
6. Final takeaway and next steps (45 sec)
