# STAT293 Final Project

## Chronic Kidney Disease (CKD) Transplant Study

**Authors:** Yijia Xue, Ying Lin\
**Course:** STAT 293\
**University:** University of California, Riverside\
**Date:** March 2026

------------------------------------------------------------------------

# Project Overview

This project investigates the association between **longitudinal
biomarker trajectories** and **renal graft failure** among kidney
transplant patients.

We apply **joint longitudinal--survival modeling** to assess how the
evolution of multiple biomarkers over time influences the risk of graft
failure. This framework simultaneously models repeated biomarker
measurements and survival outcomes while accounting for cross-biomarker
dependence.

The **primary objective** is to quantify the strength of the association
between biomarker trajectories and the hazard of renal graft failure.

A **secondary objective** is to determine **which biomarker contributes
most strongly to graft failure risk** after adjusting for the others.

------------------------------------------------------------------------

# Dataset

The dataset contains **407 renal transplant patients** with longitudinal
biomarker measurements and survival outcomes.

Four datasets are included:

  Dataset   Size         Description
  --------- ------------ -----------------------------------------
  `prot`    53,144 × 8   Proteinuria measurements (binary)
  `haem`    58,531 × 8   Haematocrit measurements (continuous)
  `gfr`     70,514 × 8   Glomerular filtration rate (continuous)
  `surv`    407 × 6      Time-to-graft failure and censoring

Common variables include:

-   id
-   age
-   weight
-   sex
-   fuyears
-   failure

The primary outcome is **time to renal graft failure**, and predictors
are **time-varying biomarker trajectories**.

------------------------------------------------------------------------

# Exploratory Data Analysis

Key observations include:

-   Median patient age: **42 years**
-   Median follow-up time: **11.9 years**
-   **126 graft failures (≈31%)**

EDA findings:

-   **Haematocrit** distribution approximately symmetric
-   **GFR** shows mild right skewness and nonlinear trends
-   **Proteinuria prevalence** varies over time
-   Significant **between-subject heterogeneity**
-   GFR and haematocrit moderately correlated (r = 0.48)

These patterns motivate the use of **mixed-effects and joint modeling
approaches**.

------------------------------------------------------------------------

# Statistical Modeling Framework

## Longitudinal Submodels

Each biomarker trajectory is modeled using mixed-effects models.

### GFR

-   Gaussian mixed-effects model
-   Nonlinear time effects modeled using natural cubic splines

### Haematocrit

-   Linear mixed-effects model
-   Random intercepts and slopes

### Proteinuria

-   Logistic mixed-effects model

Random effects are allowed to be **correlated across biomarkers**.

------------------------------------------------------------------------

## Survival Submodel

The hazard of renal graft failure is modeled using a **Cox proportional
hazards model**:

h_i(t) = h_0(t) exp{ γ\^T X_i + α1 m\_{i,gfr}(t) + α2 m\_{i,haem}(t) +
α3 m\_{i,prot}(t) }

where

-   X_i represents baseline covariates
-   m\_{i,·}(t) are predicted biomarker trajectories
-   α_k quantify biomarker--failure associations.

------------------------------------------------------------------------

# Missing Data

Approximately **27% of longitudinal biomarker measurements are
missing**.

Exploratory analyses suggest missingness depends on observed quantities
such as prior biomarker values and graft failure status.

Thus, the primary analysis assumes **Missing at Random (MAR)**.

Joint modeling helps mitigate bias from **informative dropout**.

------------------------------------------------------------------------

# Sensitivity Analysis

To evaluate robustness to possible **MNAR departures**, we conducted a
**δ-shift sensitivity analysis**.

Post-dropout GFR values were decreased by:

-   δ = 5
-   δ = 10

The joint model was then refitted to assess stability of the association
between GFR and graft failure.

------------------------------------------------------------------------

# Repository Structure

    STAT293-Final-Project
    │
    ├── STAT293_Kurum_FinalProject.Rmd
    │     Main analysis file
    │
    ├── STAT293_Final_Project_presentation.pdf
    │     Presentation slides
    │
    └── README.md
          Project documentation

------------------------------------------------------------------------

# Reproducibility Instructions

To reproduce the analysis:

1.  Clone the repository

git clone https://github.com/yijiaxue1202/STAT293-Final-Project.git

2.  Open the project in **RStudio**

3.  Install required packages

4.  Open STAT293_Kurum_FinalProject.Rmd

5.  Knit the file to reproduce the analysis and figures.

------------------------------------------------------------------------

# Software Environment

Recommended environment:

R version 4.3+

Required packages:

-   JMbayes2
-   GLMMadaptive
-   tidyverse
-   survival
-   survminer
-   splines
-   nlme
-   ggplot2

Install common packages using:

install.packages(c( "tidyverse", "survival", "survminer", "splines",
"nlme", "ggplot2" ))

------------------------------------------------------------------------

# References

Course project for **STAT 293**\
Department of Statistics\
University of California, Riverside
