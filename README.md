# Advanced Data Mining Project - Deliverables 1 and 2

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

## Files

- `deliverable1_data_collection_cleaning_exploration.ipynb`: Full notebook with code, explanations, and visualizations.
- `deliverable2_regression_modeling_performance_evaluation.ipynb`: Feature engineering, multiple regression models, cross-validation, metrics, visual comparison, and conclusions.
- `README.md`: Summary of dataset, cleaning, modeling process, insights, and challenges across deliverables.
