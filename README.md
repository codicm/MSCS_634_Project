# Advanced Data Mining Project - Deliverables 1, 2, 3, and 4

## Dataset Summary

- **Dataset**: `ecommerce_user_dataset_cleaned.csv`
- **Records**: 1,000
- **Attributes**: 8
- **Domain**: E-commerce customer behavior and segmentation
- **Key fields**:
  - `Customer_ID`
  - `Purchase_History`
  - `Transaction_Frequency`
  - `Monetary_Value`
  - `Browsing_Behavior`
  - `Engagement_Score`
  - `Time_on_Site`
  - `Customer_Segment`

This dataset is appropriate for the project because it has sufficient size and dimensionality, includes numeric behavioral features for EDA and modeling, and contains a segment label useful for future classification tasks.

## Major Steps in Data Cleaning and Exploration

1. Loaded the dataset using Pandas and inspected shape, schema, and sample records.
2. Performed quality checks for:
   - Missing values
   - Duplicate rows
   - Basic value consistency for bounded features (for example, `Engagement_Score` in [0,1])
3. Applied a reusable cleaning pipeline:
   - Column name standardization
   - Duplicate removal
   - Generic missing value imputation logic (numeric median, categorical mode)
   - Numeric type enforcement
4. Addressed noisy data using IQR-based outlier detection and capping (winsorization), mainly affecting `Transaction_Frequency`.
5. Conducted EDA with visualizations:
   - Histograms and boxplots for numeric distributions and outliers
   - Segment count plot for class balance
   - Correlation heatmap
   - Segment-wise feature mean comparison

## Key Insights

- Data quality is strong (no missing values, no duplicates in the source file).
- `Customer_Segment` is imbalanced (`Copp` ~71.8%, `Iron` ~28.2%).
- Numeric features show weak linear correlations, suggesting potential value from non-linear models and interaction features in later stages.
- Outliers are limited and mostly concentrated in `Transaction_Frequency`.
- Segment-level averages are close, indicating that additional feature engineering may be important for stronger predictive performance.

## Challenges and How They Were Addressed

- **Challenge**: The dataset appears already cleaned, which can reduce visibility into cleaning decisions.
  - **Resolution**: Implemented a robust, reusable cleaning pipeline and documented each operation so the process remains reproducible.
- **Challenge**: Weak separability between customer segments in simple summary statistics.
  - **Resolution**: Highlighted next-step modeling strategies (class balancing, non-linear methods, and engineered features) to improve downstream performance.
- **Challenge**: Potential influence of extreme values.
  - **Resolution**: Used IQR-based capping to reduce noise impact while preserving records.

## Deliverable 2: Regression Modeling and Performance Evaluation

### Objective

Predict `Monetary_Value` using engineered behavioral features and compare multiple regression models with holdout and cross-validation metrics.

### Feature Engineering Applied

- `purchase_freq_interaction` = `Purchase_History * Transaction_Frequency`
- `engagement_time_interaction` = `Engagement_Score * Time_on_Site`
- `browsing_per_visit` = `Browsing_Behavior / (Transaction_Frequency + 1)`
- `purchase_per_visit` = `Purchase_History / (Transaction_Frequency + 1)`

These engineered features were designed to capture interaction intensity and normalized behavior rates that are not visible in raw features alone.

### Regression Models Built

1. Linear Regression
2. Ridge Regression
3. Lasso Regression

### Evaluation Approach

- Holdout split: 80/20 train-test split with fixed random seed (`random_state=42`)
- Metrics: R-squared (`R2`), Mean Squared Error (`MSE`), Root Mean Squared Error (`RMSE`)
- Generalization check: 5-fold cross-validation using mean CV R2 and mean CV RMSE

### Model Comparison Summary

- The notebook generates a comparison table for all models and ranks them by cross-validation RMSE (lower is better).
- The best model is selected using CV RMSE to prioritize expected performance on unseen data.
- Residual diagnostics are included to validate prediction error patterns.

### Key Insights from Deliverable 2

- Engineered interaction and ratio features improved the model's ability to capture behavioral effects.
- Regularization methods (Ridge/Lasso) provide stability when correlated predictors exist.
- Cross-validation results are emphasized over a single split to reduce overfitting risk in model selection.

### Challenges and Mitigations

- **Challenge**: Behavioral predictors have weak simple linear relationships.
  - **Resolution**: Added interaction and normalized ratio features to increase expressive power.
- **Challenge**: Risk of selecting a model from one favorable split.
  - **Resolution**: Used 5-fold cross-validation as the primary ranking signal.

## Deliverable 3: Classification, Clustering, and Pattern Mining

### Objective

Build classification and clustering models, tune a classifier, and apply association rule mining to discover actionable behavioral patterns.

### Classification Models and Tuning

- Classification target: `Customer_Segment` (binary encoded for ROC evaluation)
- Model 1: Decision Tree Classifier
- Model 2: k-Nearest Neighbors (k-NN) Classifier
- Hyperparameter tuning: `GridSearchCV` applied to k-NN over:
  - `n_neighbors`
  - `weights`
  - `metric`

### Classification Evaluation

- Metrics reported:
  - Confusion matrix
  - ROC curve
  - Accuracy
  - F1 score
- Why F1 matters here: segment classes are imbalanced, so F1 provides a better precision/recall balance than accuracy alone.

### Clustering Model

- Clustering algorithm: K-Means (`k=3`)
- Input: standardized behavioral and engineered features
- Visualization: 2D PCA scatter plot with cluster labels
- Interpretation: cluster-level mean profile table highlights behavioral differences across groups.

### Association Rule Mining

- Technique: Apriori with association rules
- Process:
  - Discretized continuous behavior variables into quantile bins (`low/mid/high`)
  - Created transaction-like tokenized records
  - Mined frequent itemsets and high-lift rules
- Business relevance:
  - Strong rules indicate co-occurring customer behavior states
  - Rules can support personalization, recommendations, and campaign triggers.

### Key Insights from Deliverable 3

- Model comparison using F1, ROC, and confusion matrix provides stronger evidence than accuracy alone.
- Hyperparameter tuning improved classifier selection quality by using cross-validated search.
- K-Means exposed distinct customer behavior groups that can guide targeted strategy.
- Association rules surfaced interpretable patterns useful for operational decision-making.

### Challenges and Mitigations

- **Challenge**: Potential class imbalance effects on naive accuracy interpretation.
  - **Resolution**: Prioritized F1 score and ROC analysis alongside confusion matrices.
- **Challenge**: High-dimensional numeric behavior may be hard to visualize directly.
  - **Resolution**: Used PCA projection for clear cluster visualization.
- **Challenge**: Association rules require categorical transaction-like inputs.
  - **Resolution**: Applied quantile binning and tokenization before Apriori mining.

## Deliverable 4: Final Insights, Recommendations, and Presentation

### Objective

Consolidate the full project journey into final insights, practical recommendations, ethical reflection, and presentation-ready outputs.

### What Was Consolidated

- Dataset rationale and project context
- Data preparation, EDA, and feature engineering takeaways
- Regression findings (continuous outcome modeling)
- Classification findings with tuning and imbalance-aware evaluation
- Clustering insights and interpretable customer groups
- Association rule patterns for behavioral co-occurrence

### Final Insights

- Data quality checks and preprocessing produced a reliable analytical baseline.
- Feature engineering contributed meaningful signal across multiple tasks.
- Evaluation using cross-validation, F1, confusion matrix, and ROC improved reliability compared with single-metric assessment.
- Clustering and association rules added actionable segmentation and pattern-level insights beyond supervised predictions.

### Practical Recommendations

- Use model outputs and cluster profiles to design segment-specific retention and upsell workflows.
- Prioritize high-engagement but moderate-spend groups for conversion campaigns.
- Operationalize high-lift association rules as recommendation and messaging triggers.
- Monitor model and business KPIs over time and retrain periodically to manage behavior drift.

### Ethical Considerations

- **Data privacy**: exclude direct identifiers from model inputs and enforce least-privilege access.
- **Fairness/bias**: class imbalance can bias outcomes; evaluate with F1, ROC, and confusion matrix, not accuracy alone.
- **Responsible deployment**: keep human oversight for high-impact actions and avoid manipulative targeting.

### Presentation Deliverable

- Prepared a 5-7 minute presentation outline covering problem context, methods, results, ethics, and recommendations.
- The final notebook includes visuals to support each major conclusion.

## Files

- `deliverable1_data_collection_cleaning_exploration.ipynb`: Full notebook with code, explanations, and visualizations.
- `deliverable2_regression_modeling_performance_evaluation.ipynb`: Feature engineering, multiple regression models, cross-validation, metrics, visual comparison, and conclusions.
- `deliverable3_classification_clustering_pattern_mining.ipynb`: Classification, hyperparameter tuning, confusion matrix/ROC/F1 evaluation, clustering, association rules, and practical insights.
- `deliverable4_final_insights_recommendations_presentation.ipynb`: Consolidated code, final visualizations, integrated findings, ethical considerations, recommendations, and presentation outline.
- `deliverable4_comprehensive_written_report.md`: Comprehensive written report source (ready to export to PDF/Word).
- `README.md`: Summary of dataset, methods, findings, practical relevance, ethics, and challenges across all deliverables.
