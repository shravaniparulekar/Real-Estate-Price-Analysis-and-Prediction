# Real Estate Price Analysis and Prediction

## 📌 Project Overview

This project analyzes residential property characteristics and develops an interpretable **Multiple Linear Regression** model to predict house prices.

The analysis focuses on identifying the key factors associated with property prices, addressing multicollinearity, improving the distribution of the target variable, and evaluating the assumptions and performance of the regression model.

### Workflow

**EDA → Correlation Analysis → Multicollinearity Analysis → Target Transformation → Feature Selection → Multiple Linear Regression → Model Evaluation → Residual Diagnostics → Business Interpretation**

---

## 🎯 Business Problem

Real-estate prices are influenced by several property characteristics such as quality, living area, garage capacity, basement area, number of rooms, and construction history.

The objective of this project is to:

> **Understand the factors influencing house prices and build a reliable, interpretable model for estimating property sale prices.**

Such a model can support initial property valuation, pricing analysis, property comparison, and real-estate investment decisions.

---

## 🎯 Objectives

- Explore the distribution of residential property prices.
- Identify features strongly associated with `SalePrice`.
- Analyze relationships among important predictors.
- Detect multicollinearity using **Variance Inflation Factor (VIF)**.
- Transform the target variable to reduce skewness.
- Select meaningful predictors for the regression model.
- Build a **Multiple Linear Regression** model.
- Evaluate predictive performance using multiple metrics.
- Perform residual diagnostics.
- Interpret standardized regression coefficients.
- Translate statistical findings into practical real-estate insights.

---

## 📊 Dataset

The project uses a residential property dataset containing information about houses and their sale prices.

### Target Variable

`SalePrice`

### Selected Predictors

The final model uses the following variables:

| Feature | Description |
|---|---|
| `OverallQual` | Overall material and finish quality |
| `GrLivArea` | Above-ground living area |
| `GarageCars` | Garage capacity in cars |
| `GarageArea` | Garage area |
| `TotalBsmtSF` | Total basement area |
| `1stFlrSF` | First-floor area |
| `FullBath` | Number of full bathrooms |
| `TotRmsAbvGrd` | Total rooms above ground |
| `YearBuilt` | Original construction year |
| `YearRemodAdd` | Year of remodeling |

These variables capture important aspects of **property quality, size, functionality, and construction history**.

---

## 🔍 Exploratory Data Analysis

The analysis begins by examining numerical variables and their relationships with `SalePrice`.

Correlation analysis identifies the variables most strongly associated with house prices.

### Strongest Correlations with `SalePrice`

| Feature | Correlation |
|---|---:|
| `OverallQual` | ~0.79 |
| `GrLivArea` | ~0.71 |

Other important variables include:

- `GarageCars`
- `GarageArea`
- `TotalBsmtSF`
- `1stFlrSF`

The results indicate that **overall property quality and usable living space are among the strongest predictors of sale price**.

---

## 📈 Correlation and Multicollinearity

The correlation analysis also reveals strong relationships between several predictors.

Examples include:

- `GarageCars` ↔ `GarageArea` — ~0.88
- `TotalBsmtSF` ↔ `1stFlrSF` — ~0.82
- `GrLivArea` ↔ `TotRmsAbvGrd` — ~0.83

Strong relationships among predictors can lead to **multicollinearity**, which can make regression coefficients unstable and difficult to interpret.

To investigate this, **Variance Inflation Factor (VIF)** is calculated for the selected predictors.

The analysis shows that most predictors have VIF values below 5, while some exhibit moderate multicollinearity. No severe multicollinearity was identified, so the selected variables were retained for modelling.

---

## 🔄 Target Variable Transformation

The original `SalePrice` variable is strongly right-skewed.

To reduce skewness, a logarithmic transformation is applied:

```python
df["LogSalePrice"] = np.log1p(df["SalePrice"])

## 🔎 Feature Selection

The final model uses the following predictors:

```text
OverallQual
GrLivArea
GarageCars
GarageArea
TotalBsmtSF
1stFlrSF
FullBath
TotRmsAbvGrd
YearBuilt
YearRemodAdd
```

The target variable is:

```text
LogSalePrice
```

---

## 🧹 Train-Test Split

The dataset is divided into:

- **80% Training Data**
- **20% Testing Data**

A fixed random state is used to ensure reproducibility.

---

## ⚖️ Feature Scaling

The selected predictors have different units and numerical ranges.

For example:

- `OverallQual` is a rating.
- `GrLivArea` is measured in square feet.
- `YearBuilt` represents a calendar year.
- `GarageCars` represents the number of car spaces.

Therefore, the predictors are standardized using `StandardScaler`.

The scaler is fitted **only on the training data** and then applied to both the training and test sets to prevent data leakage.

---

## 🤖 Multiple Linear Regression

A **Multiple Linear Regression** model is trained to predict `LogSalePrice`.

The model can be represented as:

$$
\log(1+\text{SalePrice})
=
\beta_0 + \beta_1X_1 + \cdots + \beta_pX_p + \epsilon
$$

Since the predictors are standardized, the regression coefficients can be compared to understand their relative influence on the target.

---

## 📊 Model Evaluation

The model is evaluated using:

- $R^2$
- Adjusted $R^2$
- MAE
- MSE
- RMSE

### Performance on Log-Transformed Target

| Metric | Test Performance |
|---|---:|
| $R^2$ | ~0.844 |
| Adjusted $R^2$ | ~0.839 |

The model explains approximately **84% of the variation in the log-transformed sale price** on the test set.

---

## 💰 Performance on Original Price Scale

Since the model predicts `LogSalePrice`, predictions are transformed back to the original price scale for easier interpretation.

### Final Model Performance

| Metric | Performance |
|---|---:|
| $R^2$ | ~0.869 |
| MAE | ~20,584 |
| RMSE | ~31,736 |

The **MAE of approximately 20.6k** represents the average absolute difference between predicted and actual house prices in the dataset's currency.

The higher RMSE indicates that a smaller number of properties have relatively large prediction errors.

---

## 🔬 Residual Diagnostics

Residual analysis is performed to assess the behaviour of the regression model and identify potential issues with model assumptions.

### Residual vs Predicted Values

A well-behaved regression model should ideally have residuals randomly distributed around zero without a strong systematic pattern.

### Residual Distribution

The residual distribution is examined to assess whether the errors are reasonably centered and symmetric.

The analysis shows that most residuals are concentrated around zero, although some larger errors remain, particularly for certain high-priced properties.

---

## 📌 Feature Interpretation

The standardized regression coefficients are used to compare the relative influence of the selected predictors.

### Overall Quality

`OverallQual` has the strongest positive influence among the selected predictors.

This highlights the importance of **overall construction and material quality** in determining property value.

### Living Area

`GrLivArea` is another strong positive predictor.

Larger above-ground living areas are generally associated with higher house prices.

### Construction and Renovation

`YearBuilt` and `YearRemodAdd` show positive relationships with house price, suggesting that newer or recently renovated properties tend to command higher prices.

### Garage Features

`GarageCars` and `GarageArea` provide useful predictive information, although their high correlation means their individual coefficients should be interpreted carefully.

### Important Interpretation Note

Regression coefficients represent relationships **holding the other predictors constant**.

Therefore, a negative coefficient does not necessarily imply that increasing a feature directly reduces property value. Correlations among predictors and the conditional nature of regression coefficients should be considered when interpreting the results.

---

## 💼 Business Insights

The analysis demonstrates that a relatively small set of meaningful property characteristics can provide substantial predictive power.

### Key Insights

- **Overall property quality** is one of the strongest factors associated with sale price.
- **Above-ground living area** has a strong positive relationship with house prices.
- **Construction and renovation history** provide additional predictive information.
- Garage and area-related variables contribute useful information but may exhibit multicollinearity.

### Potential Applications

- Initial property valuation
- Pricing analysis
- Property comparison
- Market analysis
- Investment screening
- Identification of potentially under- or over-priced properties

Model predictions should be treated as **estimates rather than definitive property valuations**, particularly for properties with characteristics outside the training data.

---

## 🛠️ Tech Stack

- **Python**
- **Pandas**
- **NumPy**
- **Matplotlib**
- **Seaborn**
- **Scikit-learn**
- **Statsmodels**
- **Jupyter Notebook**

---


## 🚀 Future Improvements

Potential extensions include:

- Compare Linear Regression with **Ridge and Lasso Regression**.
- Perform systematic feature selection.
- Include relevant categorical property features.
- Investigate nonlinear relationships.
- Perform cross-validation.
- Analyze influential observations and outliers.
- Perform formal heteroscedasticity tests.
- Compare transformed and untransformed models.
- Benchmark against **Random Forest** and **Gradient Boosting**.
- Develop a simple property valuation application.

---

## 📝 Conclusion

This project demonstrates an interpretable **Multiple Linear Regression** approach to real-estate price prediction.

The analysis identifies **overall property quality, living area, garage characteristics, basement area, and construction/renovation history** as important factors associated with sale price.

The logarithmic transformation substantially reduces target skewness, while **VIF analysis and residual diagnostics** provide additional checks on the regression model.

The final model achieves approximately **86.9% $R^2$ on the original price scale**, with an **MAE of approximately 20.6k** and **RMSE of approximately 31.7k**.

Overall, the project demonstrates how **exploratory analysis, classical statistical modelling, regression diagnostics, and business interpretation** can be combined to develop an interpretable real-estate price prediction model.

