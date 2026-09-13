# Prediction Knowledge

Use this family for regression, forecasting, and supervised numerical prediction.

## Default model ladder

Consider the task and data regime before selecting models.

Typical baselines:

- mean / naive predictor,
- linear or generalized linear regression,
- classical time-series baseline when truly temporal.

Typical candidates:

- regularized regression,
- Random Forest,
- gradient boosting (XGBoost / LightGBM / CatBoost),
- support vector regression,
- neural networks when data scale and structure justify them,
- mechanism + residual hybrid models when domain equations exist.

## Key questions

- Is the task interpolation or extrapolation?
- Is there group/time leakage?
- Is the sample size sufficient?
- Are categorical variables important?
- Is interpretability required?
- Does a physical equation provide a stronger baseline?

## Validation

Prefer RMSE / MAE / R² only when appropriate. Also inspect group-wise performance, residual structure, and stability.
