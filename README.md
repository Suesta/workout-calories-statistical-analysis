# Workout Calories Statistical Analysis (R)

End-to-end statistical analysis in **R** on a fitness dataset, covering **data preprocessing**, **hypothesis testing**, **regression modeling**, **classification evaluation**, and **one-/two-way ANOVA** with **robust diagnostics**.

---

## Project Overview

This repository contains an R-based workflow that analyzes factors associated with `Calories_Burned` in workout sessions. The analysis follows a structured pipeline:

- **Preprocessing**
  - Variable name normalization and basic type inspection
  - Numeric conversion and consistency checks for `Weight_kg` and `Height_m`
  - Missing values and outlier screening (IQR-based)
- **Inferential statistics**
  - One-sided two-sample comparison (97.5% confidence level) for mean calories burned by gender
- **Modeling**
  - Multiple linear regression for `Calories_Burned`
  - Multicollinearity assessment (correlation matrix + VIF/GVIF)
  - Residual diagnostics (linearity, normality, heteroscedasticity, leverage)
  - Logistic regression for a binary target derived from `Experience_Level`
  - Confusion matrix and key classification metrics (threshold = 0.5)
- **ANOVA**
  - One-way ANOVA for `Workout_Type` with effect size (`eta^2`)
  - Assumption checks (Anderson–Darling, Levene)
  - Robust alternatives and post-hoc comparisons (Welch ANOVA + Tamhane T2 when variances differ)
  - Two-way ANOVA with `Workout_Type` and a binned BMI factor (`BMI_bin`), testing interaction and main effects

---

## Repository Structure

```

.
├── A4_2025_1_ENUN_CAST-1.pdf      # Assignment statement / guidelines
├── Findata.csv                    # Input dataset
├── Suesta_preproceso.Rmd          # Main RMarkdown analysis (code + outputs)
├── README.md                      # Project documentation
└── LICENSE                        # MIT license

````

---

## Requirements

### Software
- R (recommended: R >= 4.0)
- RStudio (optional but convenient)

### R packages
The RMarkdown includes a helper function that checks packages and stops with an install hint if missing.

Core packages used:
- `car` (VIF, Levene test)
- `caret` (confusion matrix and classification metrics)

Additional packages used in diagnostics / post-hoc:
- `nortest` (Anderson–Darling normality test)
- `PMCMRplus` (Tamhane T2 post-hoc test)
- `DescTools` (Scheffé post-hoc test; only used if homoscedasticity holds)

Install (if needed):
```r
install.packages(c("car", "caret", "nortest", "PMCMRplus", "DescTools"))
````

---

## How to Run

### Option 1 — Knit the report (recommended)

1. Open `Suesta_preproceso.Rmd` in RStudio.
2. Ensure `Findata.csv` is in the same folder as the `.Rmd`.
3. Click **Knit** (HTML output).

### Option 2 — Render from R console

```r
install.packages("rmarkdown")
rmarkdown::render("Suesta_preproceso.Rmd")
```

The rendered HTML will include all outputs: tables, tests, and diagnostic plots.

---

## Methods Summary

### 1) Preprocessing

* Column names are standardized (dots → underscores, removal of trailing punctuation, etc.).
* `Weight_kg` and `Height_m` are converted to numeric.
* Basic range-based checks are performed to identify potential unit issues.
* Missingness is preserved as `NA` (no imputation).
* Outliers are flagged using IQR limits.

### 2) Hypothesis test (Gender vs Calories_Burned)

* One-sided test at **97.5% confidence**:

  * (H_0: \mu_{Male} \le \mu_{Female})
  * (H_1: \mu_{Male} > \mu_{Female})

### 3) Multiple Linear Regression (target: Calories_Burned)

* Predictors include session duration, experience level, frequency, anthropometrics, gender, and workout type.
* Diagnostics assess:

  * Residual patterns (linearity)
  * Normality deviations
  * Heteroscedasticity signals
  * Influential observations

### 4) Logistic Regression + Confusion Matrix

* Binary target is derived from `Experience_Level` (indicator for `Experience_Level == 1`).
* Evaluation uses threshold 0.5 and reports:

  * Accuracy
  * Sensitivity / Specificity
  * Balanced accuracy
  * Kappa

### 5) ANOVA (one-way and two-way)

* One-way ANOVA compares mean `Calories_Burned` across `Workout_Type`.
* Effect size computed via (\eta^2).
* Assumptions checked:

  * Normality (Anderson–Darling)
  * Equal variances (Levene)
* Robust workflow:

  * Welch ANOVA when variances differ
  * Tamhane T2 post-hoc for unequal variances
* Two-way ANOVA:

  * Factor A: `Workout_Type`
  * Factor B: `BMI_bin` (`BMI > 25` vs `BMI <= 25`)
  * Interaction tested and removed if not significant

---

## Key Outputs Included in the Report

* Descriptive checks (missingness, summaries, outlier counts)
* Inferential test results (test statistic, df, p-value)
* Model summaries (coefficients, significance, goodness-of-fit)
* Correlation matrix + VIF/GVIF
* Residual diagnostics plots (LM and ANOVA)
* Confusion matrix and classification metrics
* ANOVA tables + robust/post-hoc comparisons

---

## Notes on Reproducibility

* Keep `Findata.csv` in the project root to run the report without path edits.
* The workflow uses deterministic procedures (no random seeds required).
* The analysis is written as an RMarkdown document to ensure transparency: code and outputs are generated together.

---

## License

This project is released under the **MIT License** (see `LICENSE`).

