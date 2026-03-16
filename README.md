# STAT293 Final Project

## Chronic Kidney Disease (CKD) Transplant Study

**Authors:** Yijia Xue, Ying Lin  
**Course:** STAT 293  
**University:** University of California, Riverside  
**Date:** March 2026

---

# Project Overview

This project investigates the relationship between **longitudinal biomarker trajectories** and the **risk of renal graft failure** among kidney transplant patients.

We apply a **joint longitudinal–survival modeling framework** to simultaneously model repeated biomarker measurements and time-to-event outcomes. This approach accounts for within-subject correlation, cross-biomarker dependence, and informative dropout.

The **primary objective** is to quantify the association between biomarker trajectories and the hazard of renal graft failure.

A **secondary objective** is to identify **which biomarker contributes most strongly to graft failure risk** after adjusting for the others.

---

# Dataset

The dataset contains **407 renal transplant patients** with repeated biomarker measurements and survival outcomes.

Four datasets are used:

| Dataset | Size | Description |
|-------|------|-------------|
| `prot` | 53,144 × 8 | Proteinuria measurements (binary) |
| `haem` | 58,531 × 8 | Haematocrit measurements (continuous) |
| `gfr` | 70,514 × 8 | Glomerular filtration rate (continuous) |
| `surv` | 407 × 6 | Time-to-graft failure and censoring |

Common variables include:

- `id`
- `age`
- `weight`
- `sex`
- `fuyears`
- `failure`

The primary outcome is **time to renal graft failure**, while predictors include **time-varying biomarker trajectories**.

The dataset is derived from the renal transplant study analyzed in:

**Fieuws et al. (2008)**, which investigated multivariate longitudinal biomarkers and graft failure risk.

---

# Exploratory Data Analysis

Key characteristics of the data include:

- Median patient age: **42 years**
- Median follow-up time: **11.9 years**
- **126 graft failures (≈31%)**

Exploratory analysis shows:

- **Haematocrit** distribution approximately symmetric
- **GFR** exhibits mild right skewness and nonlinear time trends
- **Proteinuria prevalence** varies over time
- Substantial **between-subject heterogeneity**
- Moderate correlation between GFR and haematocrit (r ≈ 0.48)

These patterns motivate the use of **mixed-effects models and joint modeling approaches**.

---

# Statistical Modeling Framework

## Longitudinal Submodels

Each biomarker trajectory is modeled using mixed-effects models.

### GFR

- Gaussian mixed-effects model  
- Nonlinear time effects modeled using **natural cubic splines**

### Haematocrit

- Linear mixed-effects model  
- Random intercepts and slopes

### Proteinuria

- Logistic mixed-effects model

Random effects are allowed to be **correlated across biomarkers**.

---

## Survival Submodel

The hazard of graft failure is modeled using a **Cox proportional hazards model**:

\[
h_i(t) = h_0(t)\exp\{\gamma^T X_i + \alpha_1 m_{i,gfr}(t) + \alpha_2 m_{i,haem}(t) + \alpha_3 m_{i,prot}(t)\}
\]

where

- \(X_i\) represents baseline covariates  
- \(m_{i,\cdot}(t)\) are predicted biomarker trajectories  
- \(\alpha_k\) quantify biomarker–failure associations.

---

# Missing Data

Approximately **27% of longitudinal biomarker measurements are missing**.

Exploratory analyses suggest that missingness depends on observed quantities such as prior biomarker values and graft failure status.

Thus, the primary analysis assumes **Missing at Random (MAR)**.

Joint modeling partially mitigates bias due to **informative dropout**, since survival and longitudinal processes are modeled simultaneously.

---

# Sensitivity Analysis

To assess robustness to potential **Missing Not At Random (MNAR)** departures, we performed a **δ-shift sensitivity analysis**.

Post-dropout GFR values were decreased by:

- δ = 5
- δ = 10

The joint model was then refitted to evaluate the stability of the estimated association between GFR and graft failure.

---

# Repository Structure
STAT293-Final-Project
│
├── STAT293_Kurum_FinalProject.Rmd
│ Main analysis file containing all modeling code
│
├── STAT293_Final_Project_presentation.pdf
│ Final presentation slides
│
└── README.md
Project documentation


---

# Reproducibility Instructions

To reproduce the analysis:

1. Clone the repository
git clone https://github.com/yijiaxue1202/STAT293-Final-Project.git


2. Open the project in **RStudio**

3. Ensure the dataset `ckd.rdata` is placed in the project directory  
   (the dataset is provided through the course materials)

4. Install required R packages

5. Open `STAT293_Kurum_FinalProject.Rmd`

6. Knit the R Markdown file to reproduce the analysis, tables, and figures.

---

# Software Environment

Recommended environment:

- **R version:** 4.3 or higher

Required packages:

- JMbayes2
- GLMMadaptive
- tidyverse
- survival
- survminer
- splines
- nlme
- ggplot2

Install packages using:

```r
install.packages(c(
  "tidyverse",
  "survival",
  "survminer",
  "splines",
  "nlme",
  "ggplot2"
))
```
# References
Fieuws, S., Verbeke, G., Maes, B., & Vanrenterghem, Y. (2008). Predicting renal graft failure
using multivariate longitudinal profiles. Biostatistics, 9(3), 419–431.

Rizopoulos, D. (2012). Joint Models for Longitudinal and Time-to-Event Data: With Applica-
tions in R. Chapman & Hall/CRC.

Rizopoulos, D. (2021). JMbayes2: Extended Joint Models for Longitudinal and Time-to-Event
Data. R package documentation.
