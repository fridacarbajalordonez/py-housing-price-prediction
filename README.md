# California Housing Price Prediction

Hero image (opcional)

## 1. Project Overview
This project develops and compares two Linear Regression models to predict California housing prices using demographic, geographic, and socioeconomic variables.  
The objective is to evaluate the performance of a Linear Regression model and analyze the impact of data cleaning on prediction accuracy.

The project follows a complete Machine Learning workflow, including data exploration, preprocessing, model development, evaluation, and model comparison.

Main skills demonstrated:

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Linear Regression
- Exploratory Data Analysis (EDA)
- Model Evaluation
- Data Cleaning
- Data Visualization

## 2. Business Problem

Accurately estimating housing prices is valuable for:

- Real estate agencies
- Property investors
- Financial institutions
- Home buyers

The goal is to estimate the median house value using demographic and geographical variables.

## 3. Dataset
In this analysis we are using a California Housing Dataset provided by scikit-learn for building a Linear Regression Model.
The dataset contains 20,640 observations (instances) and 8 predictor variables (attributes) describing demographic and housing characteristics of California districts.
 The target variable (Price) represents the median house value expressed on thousands of dollars ($100,000).

| Attribute  | Description                              |
|------------|------------------------------------------|
| MedInc     | median income in block group             |
| HouseAge   | median house age in block group          |
| AveRooms   | average number of rooms per household    |
| AveBedrms  | average number of bedrooms per household |
| Population | block group population                   |
| AveOccup   | average number of household members      |
| Latitude   | block group latitude                     |
| Longitude  | block group longitude                    | 



## 4. Project Workflow

![workflow1.png](images/workflow1.png)


Instead of training a single Linear Regression model, this project compares two models trained under different data preparation strategies to evaluate the impact of right-censored observations on predictive performance.

### 4.1 Exploratory Data Analysis
![correlation_heatmap.png](images/correlation_heatmap.png)
The correlation matrix shows that the strongest correlation for Price exists with MedInc attribute.  
![pairplot.png](images/pairplot.png)
The pairplot illustrates the distribution of each variable and the pairwise relationships among the predictor variables.
![original_price.png](images/original_price.png)
The Price distribution shows House prices above the upper limit are right-censored and recorded as the maximum value of 5.
This might generate bias and affect the accuracy of data for building the model.
Thus, building two models would be preferible in order to evaluate if this right-censoring problem affects significantly to the predictive model.

### 4.2 Data Cleaning Strategy
In order to handle the right-censoring problem, we remove observations where Price is equal to the upper limit (5).
The two models are build following the same model development for making the results comparable.

| Model                              | Description                              |
|------------------------------------|------------------------------------------|
| Original Dataset Linear Regression | Uses the original California Housing dataset.             |
| Cleaned Dataset Linear Regression  | Removes right-censored observations before training.         |

## 5. Model Development
The Linear Regression Model is applied to both original and cleaned datasets in the same method:
- Define independent predictor **X** variables and dependent **Y** target variable
- Split the data sample into training and testing sets using the same random seed and train-test proportion to make it comparable to previous model.
- Use the  `class sklearn.linear_model.LinearRegression()` to estimate the intercept and the regression coefficients that best describe the relationship between the predictor variables and the target variable by minimizing the residual sum of squares.

## 6. Results

### Predicted vs Actual Plot
![actual_vs_predicted_LM1.png](images/actual_vs_predicted_LM1.png)
This plot shows that predictions obtained from the model built over the original dataset don´t match the real values

![actual_vs_predicted_LM2.png](images/actual_vs_predicted_LM2.png)
On the other hand, predictions obtained from the model built over the clean dataset have a better approach to real data.

### Residuals Histogram 
![residualsLM1.png](images/residualsLM1.png)
Histogram of residuals in the original dataset model are not nearly normally distributed and centered around zero, indicating poor predictability in this model.

![residualsLM2.png](images/residualsLM2.png)
For the clean dataset model we can appreciate that residuals are evenly distributed and centered into zero, which is the desired behaviour for residuals.

### Regression Evaluation Metrics
The Regression Evaluation Metrics were computed for both models. Table bellow shows the result for each model by metric.

| Metric | Original Dataset | Clean Dataset |
|--------|------------------|---------------|
| MAE    | 0.562056         | 0.476551      |
| MSE    | 6.160635         | 0.411455      |
| RMSE   | 2.482063         | 0.641448      |

Clean Dataset metrics are significantly lower than Original Dataset metrics. This indicates that removing the observations reduces the bias and increases the accuracy of prediction for our model.

## 7. Key Findings 
- Removing right-censored observations substantially improved prediction accuracy.
- The cleaned model produced smaller prediction errors.
- Residuals became more symmetrically distributed.
- Data quality had a stronger impact than modifying the regression algorithm.


## 8. Future Improvements
Possible extensions of this project include:

- Improve data: Feature Engineering  to create additional explanatory variables.
- Improve the evaluation: Evaluate model robustness using K-Fold Cross Validation instead of a single train-test split.
- Improve the model: Compare the Linear Regression baseline with more advanced regression algorithms such as Random Forest and Gradient Boosting.
- Improve the statistical approach: Investigate statistical methods specifically designed for censored target variables instead of removing censored observations.


## 12. About the Author
Developed by Frida Elizabeth Carbajal Ordóñez.

Actuarial Science graduate with an interest in data science, analytics, and data engineering. Passionate about transforming raw data into meaningful insights through SQL, Python, and data visualization.