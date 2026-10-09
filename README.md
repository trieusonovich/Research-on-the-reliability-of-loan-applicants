# Scoring Data Analysis Project

## 📋 Overview

This project analyzes the **scoring_data.csv** dataset — credit scoring data of customers, including demographic information, employment status, income, and loan purpose.

## 📁 Data Structure

The `scoring_data.csv` file contains the following columns:

| Column | Description |
|--------|-------------|
| `children` | Number of children |
| `days_employed` | Number of days employed (negative values) |
| `dob_years` | Customer's age |
| `education` | Education level |
| `education_id` | Encoded education level |
| `family_status` | Marital status |
| `family_status_id` | Encoded marital status |
| `gender` | Gender (M/F) |
| `income_type` | Income type (occupation) |
| `debt` | Has debt or not (0/1) |
| `total_income` | Total income |
| `purpose` | Loan purpose |

## 🎯 Analysis Objectives

- **Analyze repayment capability** of customers based on demographic and financial characteristics.
- **Explore relationships** between education level, marital status, income type, and debt behavior.
- **Build predictive models** for customer debt behavior.
- **Clean the data** — handle missing values and inconsistent data (e.g., `children` = 20, `dob_years` = 0).

## 🧹 Data Cleaning

Some issues to address in the dataset:

- **Missing values**: Many columns such as `days_employed` and `total_income` have missing entries.
- **Inconsistent data**:
  - `children` has abnormal values (20).
  - `dob_years` has a value of 0.
  - `education` has inconsistent casing (`высшее`, `Высшее`, `ВЫСШЕЕ`).
  - `gender` contains the value `XNA`.
- **Numeric format**: `days_employed` is stored as negative numbers and may need normalization.

## 📊 Proposed Analysis

1. **Descriptive Statistics** (EDA):
   - Distribution of age, income, and number of children.
   - Debt ratio by gender, education level, and marital status.

2. **Visualization**:
   - Histograms for `total_income` and `dob_years`.
   - Boxplots comparing income by `income_type`.
   - Bar charts of debt ratio by `purpose`.

3. **Modeling**:
   - Binary classification to predict `debt` (0/1).
   - Suggested algorithms: Logistic Regression, Random Forest, Gradient Boosting.

## 🛠️ Tools Used

- Python (Pandas, NumPy, Matplotlib, Seaborn, Scikit-learn)
- Jupyter Notebook

## 📌 Notes

- The dataset contains many Russian-language values and should be normalized before analysis.
- Outliers in `total_income` and `days_employed` should be carefully examined.

## 👤 Author

- **Project**: Scoring Data Analysis
- **Purpose**: Learning and research in credit data analysis.

---

*This README was created based on a template, with the Getting Started section omitted as requested.*
