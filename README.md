# 🏠 House Price Forecasting — Multiple Linear Regression

> Predicting house sale prices using OLS solved analytically via matrix algebra, validated against `statsmodels`.

---

## 📌 Overview

This project builds a **Multiple Linear Regression model** to forecast residential sale prices based on observable property features. The core goal is not just to predict, but to **understand the math behind the algorithm** — solving OLS analytically using the matrix formula:

$$\hat{\beta} = (X'X)^{-1}X'Y$$

Results are then validated against `statsmodels.OLS` to confirm mathematical equivalence.

---

## 🎯 Target Variable

| Variable | Description |
|----------|-------------|
| `price` | Sale price of a house in USD |

---

## 📊 Dataset & Feature Selection

Starting from the raw dataset, the following variables were **excluded upfront**:

| Variable | Reason |
|----------|--------|
| `id`, `date` | Identifiers with no predictive value |
| `zipcode` | Categorical with no linear scale |
| `lat`, `long` | Raw coordinates — no direct linear relationship |
| `sqft_above` | Collinear with `sqft_living` |
| `yr_renovated` | ~96% of values are zero |

The **11 candidate variables** that entered the statistical selection process:

| Variable | Description |
|----------|-------------|
| `bedrooms` | Number of bedrooms |
| `bathrooms` | Number of bathrooms |
| `sqft_living` | Living area (ft²) — highest individual correlation with price |
| `sqft_lot` | Lot size (ft²) |
| `floors` | Number of floors |
| `waterfront` | Waterfront view (0/1) |
| `view` | View quality (0–4) |
| `condition` | Property condition (1–5) |
| `grade` | Construction quality (1–13) |
| `sqft_basement` | Basement area (ft²) |
| `yr_built` | Year built |

---

## 🔁 Backward Elimination (α = 0.05)

Starting from the full 11-variable model, variables were iteratively removed if their p-value exceeded **0.05**.

| Iteration | Action |
|-----------|--------|
| **1** | Remove `sqft_basement` — p-value = 0.3967 ❌ |
| **2** | All remaining variables significant (p < 0.0001) ✅ — **STOP** |

**Final model: 10 variables**, all with p-value < 0.0001.

---

## 📐 Final Model Coefficients

| Variable | Coefficient | t-stat | 95% CI |
|----------|-------------|--------|--------|
| Intercept | 6,216,011.72 | 41.58 | [5,922,968 — 6,509,054] |
| `bedrooms` | −34,441.54 | −14.90 | [−38,972 — −29,910] |
| `bathrooms` | 44,274.79 | 10.79 | [36,232 — 52,317] |
| `sqft_living` | 162.91 | 48.50 | [156.33 — 169.49] |
| `sqft_lot` | −0.24 | −5.90 | [−0.317 — −0.159] |
| `floors` | 27,512.42 | 6.24 | [18,872 — 36,152] |
| `waterfront` | 563,462.16 | 28.19 | [524,286 — 602,638] |
| `view` | 43,372.89 | 15.93 | [38,037 — 48,708] |
| `condition` | 16,625.68 | 5.69 | [10,895 — 22,355] |
| `grade` | 123,645.98 | 44.24 | [118,167 — 129,124] |
| `yr_built` | −3,576.88 | −46.89 | [−3,726 — −3,427] |

---

## ✅ Model Evaluation

| Metric | Value |
|--------|-------|
| R² — Training (70%) | **65.1%** |
| R² — Test (30%) | **65.3%** |
| F-statistic | **2,566** |
| Final variables | **10** |

The near-identical R² between training and test sets confirms **no overfitting**.  
Residuals on the test set are approximately normally distributed, supporting model assumptions.

---

## 🔬 Matrix vs. OLS Validation

Coefficients computed manually with NumPy using β = (X'X)⁻¹X'Y are **identical** to those produced by `statsmodels.OLS` (difference < 10⁻⁴ across all variables), confirming a deep understanding of the mathematical foundations of linear regression.

---

## 💡 Key Findings

- **Quality > Size**: While `sqft_living` matters, `waterfront` (+$563K) and `grade` (+$123K per level) have far greater economic impact.
- **Year built is a strong depressor**: Each additional year of age reduces estimated price by ~$3,577.
- **Bedrooms have a negative coefficient** when controlling for other variables — a larger number of rooms in a fixed living area signals smaller individual spaces.
- **`sqft_basement` adds no value** once other size variables are included — removed in iteration 1.

---

## 🛠️ Tech Stack

![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)
![NumPy](https://img.shields.io/badge/NumPy-matrix_algebra-013243?logo=numpy)
![Pandas](https://img.shields.io/badge/Pandas-data_wrangling-150458?logo=pandas)
![statsmodels](https://img.shields.io/badge/statsmodels-OLS_validation-green)
![Matplotlib](https://img.shields.io/badge/Matplotlib-visualization-orange)

---

## 📁 Project Structure

```
house-price-regression/
│
├── data/
│   └── kc_house_data.csv          # King County house sales dataset
│
├── notebooks/
│   └── house_price_regression.ipynb  # Full analysis & model development
│
├── outputs/
│   └── Resumen_Pronostico_Casa.pdf   # Project summary report
│
└── README.md
```

---

## 👤 Author

**Ulises Abraham Ortiz Sánchez**  
[![LinkedIn](https://img.shields.io/badge/LinkedIn-uliabraham-blue?logo=linkedin)](https://linkedin.com/in/uliabraham)
[![Kaggle](https://img.shields.io/badge/Kaggle-uliabraham-20BEFF?logo=kaggle)](https://www.kaggle.com/uliabraham)
[![Portfolio](https://img.shields.io/badge/Portfolio-lukasezequiel55-black?logo=google-chrome)](https://portafoliocv.lukasezequiel55.com)
