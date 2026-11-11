# California Housing Price Prediction

## 1. Project Overview

This project explores the use of machine learning to predict the median house value of California districts using demographic, housing, and geographic features.

The main objective is to investigate how data quality, feature engineering, and model selection affect predictive performance, with particular attention to two challenges:

- **Right-censored target values:** House values above $500,000 are recorded at the $500,000 cap.
- **Extreme feature outliers:** Some census-derived variables contain implausible values that can significantly affect model performance.

The project follows a complete machine learning workflow, from exploratory data analysis and data-quality investigation to model comparison, evaluation, and interpretation.

## 2. Dataset

The project uses the California Housing dataset from Scikit-learn.

- **20,640 observations**
- **8 original predictive features**
- Target variable: `Price`, representing median house value in units of $100,000.

The predictors include median income, house age, average rooms and bedrooms, population, average occupancy, latitude, and longitude.

## 3. Project Workflow

### 3.1 Exploratory Data Analysis (EDA)

Conducted an exploratory analysis to understand the dataset structure, feature distributions, correlations, geographic patterns, and potential data-quality issues.

Key findings included:

- Right-censoring of house values at $500,000.
- Extreme values in census-derived features such as `AveRooms`, `AveBedrms`, and `AveOccup`.
- Strong geographic relationships between latitude, longitude, and housing prices.
- Potential multicollinearity among certain predictors.

### 3.2 Experimental Design and Data Quality

Designed a consistent evaluation strategy to ensure fair comparisons between models.

- Created one fixed 80/20 train/test split using a fixed random seed.
- Reused the same test set across all experiments.
- Learned outlier-clipping thresholds exclusively from training data to prevent data leakage.
- Evaluated models using RMSE, MAE, and R².
- Also reported RMSE on the uncensored portion of the test set.

### 3.3 Linear Regression Experiments

Compared four configurations using Linear Regression to investigate the effects of target censoring and feature outliers:

| Experiment | Training data | Feature treatment |
|---|---|---|
| A. Baseline | All observations | Original features |
| B. Drop censored | Uncensored observations only | Original features |
| C. Clip outliers | All observations | Upper clipping at the 99th percentile |
| D. Both | Uncensored observations only | Upper clipping at the 99th percentile |

A custom Scikit-learn transformer was implemented to learn clipping thresholds from the training data.

**Key finding:** Removing censored observations had almost no effect on performance, while clipping extreme feature values reduced RMSE by approximately 8.9% compared with the baseline.

This suggested that extreme predictor values were a more significant source of error for the initial linear model than target censoring.

### 3.4 Feature Engineering

Created three additional features to capture relationships that were not fully represented by the original predictors:

- `BedroomRatio`: Proportion of average bedrooms relative to average rooms.
- `RoomsPerPerson`: Average rooms per household member, representing crowding.
- `DistNearestCity`: Distance in kilometers to the nearest of five major California cities.

Geographic distances were calculated using the **Haversine formula**, based on latitude and longitude.

All engineered features were calculated using information from the same observation, without using information from other rows.

### 3.5 Model Comparison and Cross-Validation

Compared four regression algorithms using both the original and engineered feature sets:

- Linear Regression
- Ridge Regression
- Random Forest Regressor
- Histogram-based Gradient Boosting Regressor

Used **5-fold cross-validation on the training set** to compare model performance while keeping the test set untouched.

The evaluation showed that tree-based ensemble models substantially outperformed linear models.

**Best configuration:** Histogram-based Gradient Boosting with engineered features.

- Cross-validation RMSE: **0.4619**
- Cross-validation R²: **0.8403**

Feature engineering improved all evaluated models, although the choice of algorithm had a larger impact on predictive performance.

### 3.6 Final Model Evaluation

The best configuration selected through cross-validation was refitted on the full training set and evaluated on the untouched test set.

**Final model: Histogram-based Gradient Boosting with engineered features.**

| Metric | Baseline Linear Regression | Final Model |
|---|---:|---:|
| RMSE | 0.7456 | 0.4570 |
| R² | 0.5758 | 0.8406 |
| MAE | ~$53,320 | ~$30,069 |

The final model achieved:

- **38.7% reduction in RMSE**
- **43.6% reduction in MAE**
- **Improvement in R² from 0.5758 to 0.8406**

### 3.7 Model Interpretation and Error Analysis

Evaluated the final model through:

- Predicted vs. observed values.
- Residual distribution analysis.
- Mean absolute error across different price ranges.
- Permutation feature importance.

The analysis showed that:

- Most predictions were relatively close to the ideal prediction line.
- Residuals were concentrated around zero, with a slight right-skewed tail.
- The model tended to overestimate lower-priced districts and underestimate the most expensive ones.
- Absolute prediction errors increased with house prices.
- Latitude, longitude, and median income were the most influential predictors.
- Among the engineered features, distance to the nearest major city contributed the most.

## 4. Key Conclusions

1. **Data quality matters:** Extreme values in the predictor variables significantly affected the initial linear regression model. Clipping these values improved performance more than removing censored target observations.

2. **Model selection matters:** Tree-based ensemble methods captured nonlinear relationships and interactions more effectively than linear regression.

3. **Feature engineering adds value:** Geographic distance and housing-related ratios improved cross-validated performance across the evaluated algorithms.

4. **Model performance is not uniform:** Higher-priced districts have larger absolute errors, while the target cap limits the ability to evaluate the true value of expensive properties.

5. **Business relevance:** The final model reduced the average absolute prediction error from approximately $53,320 to $30,069, providing more accurate estimates for district-level housing values.

## 5. Limitations and Future Improvements

- The dataset represents information collected around 1990. Inflation and more recent economic indicators would be necessary for contemporary price estimation.
- The target variable is capped at $500,000, limiting the reliability of predictions for high-value districts. Censored regression approaches, such as Tobit models, could be explored.
- The dataset contains district-level aggregates, so predictions represent median district values rather than individual property prices.
- Further hyperparameter tuning and additional feature exploration could improve model performance.

## 6. Technologies and Skills

**Python | Pandas | NumPy | Matplotlib | Seaborn | Scikit-learn | Jupyter Notebook**

Key skills demonstrated:

- Exploratory Data Analysis
- Data Quality Assessment
- Outlier Detection and Treatment
- Feature Engineering
- Regression Modeling
- Cross-Validation
- Model Evaluation and Selection
- Residual Analysis
- Permutation Feature Importance
- Statistical Interpretation and Data-Driven Decision Making