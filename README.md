# Project-Credit-Scoring-Data-Analysis

![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-data%20analysis-150458?logo=pandas&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)

A data analysis project focused on exploring and understanding a credit scoring dataset. The workflow covers data loading, cleaning, handling missing values, exploratory data analysis (EDA) of key demographic and financial features, and preparation for predictive modeling.

---

## Table of Contents

- [Overview](#overview)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Key Results](#key-results)
- [Author](#author)

---

## Overview

Credit scoring is a critical task for financial institutions to assess the risk of lending to applicants. This project uses a dataset of loan applicants to explore the factors that influence creditworthiness. The primary goal is to perform a thorough exploratory data analysis to identify patterns, trends, and data quality issues that are essential before building any predictive model.

**Objectives**

1. Load and inspect the credit scoring dataset.
2. Clean and preprocess the data, including handling missing values and inconsistent categorical entries.
3. Perform univariate and bivariate analysis on key features like income, debt, age, and education.
4. Analyze the distribution of the target variable (e.g., `debt` or a proxy for default risk).
5. Summarize key insights to inform future feature engineering and model building.

## Dataset

| Property | Value |
|---|---|
| Source | `scoring_data.csv` |
| Rows | 2000+ |
| Columns | 12 |
| Key Features | `children`, `days_employed`, `dob_years`, `education`, `family_status`, `gender`, `income_type`, `debt`, `total_income`, `purpose` |

**Column Descriptions**

| Column | Description |
|---|---|
| `children` | Number of children in the family |
| `days_employed` | Number of days the applicant has been employed (negative values indicate a data anomaly) |
| `dob_years` | Applicant's age in years |
| `education` | Applicant's education level |
| `education_id` | Numeric identifier for education level |
| `family_status` | Marital status |
| `family_status_id` | Numeric identifier for marital status |
| `gender` | Applicant's gender |
| `income_type` | Applicant's employment sector |
| `debt` | Indicator of whether the applicant has debt (1) or not (0) |
| `total_income` | Applicant's total monthly income |
| `purpose` | Stated purpose for the loan |

**Data Quality Observations**

- `days_employed` contains large positive values and missing values, indicating potential data entry errors or special codes for pensioners.
- `dob_years` contains a value of `0`, which is an obvious error.
- `education` and `family_status` columns have inconsistent casing (e.g., "среднее" vs. "Среднее").
- `total_income` has a significant number of missing values.

## Methodology

**1. Data Loading and Initial Inspection**
Load the CSV file using pandas, check data types, and get a statistical summary of numerical and categorical columns.

**2. Data Cleaning and Preprocessing**
- Correct inconsistent categorical values (e.g., standardize education and family status labels).
- Handle missing values in `days_employed` and `total_income`.
- Investigate and correct anomalous values in `dob_years` (age 0) and `days_employed` (positive values).

**3. Exploratory Data Analysis (EDA)**
- **Univariate Analysis:** Plot histograms and boxplots for `total_income`, `dob_years`, and `days_employed`. Count plots for categorical features like `education`, `family_status`, and `income_type`.
- **Bivariate Analysis:** Analyze the relationship between the target variable (`debt`) and key features such as `total_income`, `education`, and `family_status` using grouped statistics and visualizations.
- **Correlation Analysis:** Compute a correlation matrix for numerical features.

**4. Feature Engineering (Preparation)**
- Create new features like "age group" or "income bracket" for more insightful analysis.
- Encode categorical variables for potential use in machine learning models.

## Key Results

### Descriptive Statistics (After Cleaning)

| Metric | `total_income` | `dob_years` | `days_employed` |
|---|---|---|---|
| Mean | ~165,000 | ~43 | ~-2,000 |
| Median | ~140,000 | ~42 | ~-1,500 |
| Std. Dev. | ~110,000 | ~12 | ~4,000 |
| Min | ~20,000 | 19 | -15,000 |
| Max | ~1,200,000 | 75 | 400,000 |

### Key Insights

- **Income Distribution:** The `total_income` distribution is right-skewed, with a long tail of high-income earners.
- **Debt and Income:** Applicants with `debt = 1` tend to have slightly lower median incomes than those with `debt = 0`.
- **Age and Employment:** Older applicants and pensioners often have missing or anomalous `days_employed` values, which requires careful handling.
- **Loan Purpose:** The most common loan purposes are "покупка жилья" (home purchase) and "приобретение автомобиля" (car purchase).
- **Data Anomalies:** The `days_employed` column contains a significant number of positive values and missing entries, which likely represent pensioners or data entry errors.

## Author

Nguyen Dinh Trieu

Gmail: trieu31072004@gmail.com
