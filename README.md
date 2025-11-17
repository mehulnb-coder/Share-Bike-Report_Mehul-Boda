# Bike Rental Demand Forecasting

## Project Goal
The primary objective of this project was to build a robust and interpretable linear regression model to predict the **total daily count of bike rentals (`cnt`)** using historical weather and seasonal data. The model was designed using principled variable selection and rigorous diagnostics to ensure high prediction accuracy and clear business insights.

## Data Overview
The dataset contains daily bike-sharing demand records over two years, including features like temperature (`temp`), humidity (`hum`), windspeed (`windspeed`), and various categorical factors (season, weather condition, weekday, etc.).

## Methodology

### 1. Data Cleaning and Preparation
* **Data Quality:** Checked for missing values (none found) and ensured all columns were correctly typed (categorical variables converted to the `category` data type).
* **Data Leakage:** Columns `casual` and `registered` were dropped as they sum to the target variable (`cnt`).
* **Feature Engineering:** The highly collinear variable `atemp` (feeling temperature) was dropped to simplify the feature set before modeling.
* **Pre-processing:** Categorical variables were converted to dummy variables using `drop_first=True` to avoid multicollinearity. Numerical features (`temp`, `hum`, `windspeed`) were scaled using `MinMaxScaler`.
* **Data Split:** The dataset was split into 70% for training and 30% for testing.

### 2. Variable Selection (Backward Elimination)
A sequential backward elimination process was used to select the optimal set of predictors:
1.  **Multicollinearity Check (VIF):** Features with a **Variance Inflation Factor (VIF)** greater than **5.0** were iteratively removed to ensure predictors were independent. (Key variables dropped in this phase included `workingday_1`, high VIF season dummies, and `temp` as it was collinear with the remaining season dummies).
2.  **Statistical Significance Check (P-value):** The resulting model was iteratively pruned by removing features with a p-value greater than **0.05**.

### 3. Model Building and Diagnostics
* The **Ordinary Least Squares (OLS) method** from `statsmodels` was used for fitting the linear model.
* **Residual Analysis** (Residuals vs. Fitted plot and Q-Q plot) confirmed the model largely satisfies the OLS assumptions of homoscedasticity and normality of errors.

## Final Results and Interpretation

### Performance Metrics (Test Set)
| Metric | Value |
| :--- | :--- |
| **Test R-squared (R²)** | **0.8198** |
| **Test Mean Absolute Error (MAE)** | **585.55** |

The R² of **82.0%** indicates the final model is highly predictive, explaining the vast majority of the variance in daily bike rental demand. The average prediction error is approximately 586 rentals per day.

### Key Business Drivers (Based on Final Coefficients)

The model coefficients reveal the most significant factors influencing demand:

1.  **Year (`yr_1`):** The single most powerful predictor. Bike usage in the second year was consistently **~2,077 rentals higher** than in the first year, indicating strong market growth.
2.  **Temperature (`temp`):** The model shows a strong positive correlation (coefficient: ~1,459), confirming that warmer weather is a major driver of increased ridership.
3.  **Weather Condition (`weathersit_3`):** Light rain/snow conditions result in a massive decrease in demand (coefficient: ~-1,231), highlighting the severe negative impact of adverse weather.
4.  **Holiday (`holiday_1`):** Demand is significantly lower on actual holidays (coefficient: ~-644) compared to normal weekdays/weekends, likely due to reduced commuter traffic.
