# STAT293 Final Project

## Chronic Kidney Disease (CKD) Transplant Study

**Authors:** Yijia Xue, Ying Lin
**Course:** STAT 293
**Date:** March 2026

---

## Project Overview

This project investigates the association between **longitudinal biomarker trajectories** and **renal graft failure** among kidney transplant patients.

We apply **joint longitudinal–survival modeling** to assess how the evolution of multiple biomarkers over time influences the risk of graft failure. This framework allows us to simultaneously model repeated biomarker measurements and survival outcomes while accounting for cross-biomarker dependence.

The primary objective is to quantify the strength of the association between biomarker trajectories and the hazard of renal graft failure. 

A secondary objective is to determine **which biomarker contributes most strongly to graft failure risk** after adjusting for the others. 

---

## Dataset

The dataset contains **407 renal transplant patients** with longitudinal biomarker measurements and survival outcomes. 

Four datasets are included:

| Dataset | Size       | Description                             |
| ------- | ---------- | --------------------------------------- |
| `prot`  | 53,144 × 8 | Proteinuria measurements (binary)       |
| `haem`  | 58,531 × 8 | Haematocrit measurements (continuous)   |
| `gfr`   | 70,514 × 8 | Glomerular filtration rate (continuous) |
| `surv`  | 407 × 6    | Time-to-graft failure and censoring     |

Common variables across datasets include:

* `id`
* `age`
* `weight`
* `sex`
* `fuyears` (follow-up time)
* `failure` (graft failure indicator)

The primary outcome is **time to renal graft failure**, and the primary predictors are **time-varying biomarker trajectories**. 

---

## Exploratory Data Analysis

Key observations from preliminary analysis include:

* Median patient age: **42 years** (IQR 31–52)
* Median follow-up time: **11.9 years**
* **126 graft failures (≈31%)** observed during follow-up. 

Important EDA findings:

* **Haematocrit** distribution is approximately symmetric.
* **GFR** shows mild right skewness and potential nonlinear trends over time.
* **Proteinuria prevalence** varies across follow-up time.
* Substantial **between-subject heterogeneity** exists in biomarker trajectories.
* Mean GFR and haematocrit are moderately correlated *(r = 0.48)*. 

These patterns motivate the use of **mixed-effects models and joint modeling approaches**.

---

## Statistical Modeling Framework

### Longitudinal Submodels

Each biomarker trajectory is modeled using a mixed-effects framework:

**GFR (continuous)**

* Gaussian mixed-effects model
* Nonlinear time effects modeled using **natural cubic splines**

**Haematocrit (continuous)**

* Linear mixed-effects model
* Random intercepts and slopes

**Proteinuria (binary)**

* Logistic mixed-effects model

Random effects are allowed to be **correlated across biomarkers** to capture cross-biomarker dependence.

---

### Survival Submodel

The hazard of renal graft failure is modeled using a **Cox proportional hazards model** linked to the predicted biomarker trajectories:

$$
h_i(t) = h_0(t)\exp{\gamma^\top X_i + \alpha_1 m_{i,gfr}(t) + \alpha_2 m_{i,haem}(t) + \alpha_3 m_{i,prot}(t)}
$$

where

* (X_i) represents baseline covariates
* (m_{i,\cdot}(t)) are the estimated biomarker trajectories
* (\alpha_k) quantify the association between biomarkers and graft failure risk. 

---

## Missing Data

Approximately **27% of longitudinal measurements are missing**. 

Exploratory analyses suggest missingness depends on observed quantities such as graft failure status and prior GFR values.

Therefore, the primary analysis assumes **Missing at Random (MAR)** and uses a joint modeling framework to reduce bias from informative dropout.

---

## Methodological Extension

This project extends standard course methods by implementing a **multivariate joint longitudinal–survival model** that:

* simultaneously models multiple correlated biomarkers
* incorporates **nonlinear time effects using spline modeling**. 

## Sensitivity Analysis

To test the robustness of **MAR** mechanism and considering a possible **MNAR** depatures, simple $\delta$ shift sensitivity analysis is performed.
Post drop-out GFR values are downward shifted by $\delta=5$ and $\delta=10$, and the joint model is then refit under these perturbed scenarios.

---

## Software

The analysis is conducted in **R**, using packages such as:

* `JMbayes2`
* `GLMMadaptive`
* `tidyverse`
* `survival`
* `survminer`
* `splines`
* `nlme`
* `ggplot2`

---

## References

Course project for **STAT 293**, University of California, Riverside.
