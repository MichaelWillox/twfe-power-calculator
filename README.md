# TWFE Panel Data Power & Linkage Calculator

This repository contains an interactive statistical power and sample size calculator for Two-Way Fixed Effects (TWFE) econometric regressions. It is specifically designed to evaluate linked economic, energy, and environmental expenditure microdata panels, illustrating the statistical constraints ("intersection penalties") of tracking firms over time.

**[Launch the Interactive TWFE Power Calculator](https://michaelwillox.github.io/twfe-power-calculator/)**

## Key Abbreviations
* **TWFE (Two-Way Fixed Effects):** An econometric panel data model that controls for both unobserved unit-specific traits (e.g., firm fixed effects) and time-specific shocks (e.g., year fixed effects).
* **MDE (Minimum Detectable Effect):** The smallest true causal effect size (here, $\beta_1$) that the study is statistically powered to detect reliably without mistaking background noise for a real impact.
* **DEFF (Design Effect):** A multiplier applied to the baseline sample size to account for the non-independence of observations, primarily driven by within-firm serial correlation across the panel years.
* **FEs (Fixed Effects):** Categorical dummy variables (or within-transformations) used to absorb constant structural differences across entities (e.g., industries, provinces) or time periods.

## Input Parameters Definitions

### Core Survey & Model Assumptions
* **Target MDE ($\beta_1$, % Change in $Y$):** The hypothetical true effect size you want to detect. For example, if investing in clean technology boosts productivity by 2%, the target is 2.0%. 
* **CleanTech Treatment Share ($p$):** The proportion of firms in your linked sample that actually invested in the treatment (e.g., environmental expenditures). Maximum statistical power is achieved at 50%.
* **Baseline Unconditional Variance ($\sigma^2_{\ln Y}$):** The maximum variance of your outcome variable before accounting for any inputs or fixed effects.
* **Variance Explained by $E, L, K$ & FEs ($R^2$):** The proportion of the baseline variance that is successfully absorbed by your standard production inputs (Energy, Labor, Capital) and your fixed effects. A higher $R^2$ lowers residual noise.

### Panel Structure
* **Balanced Panel Horizon ($T$ Years):** The exact number of consecutive years a firm must appear in all linked surveys to be included in the analytical sample. 
* **Within-Firm Residual Autocorrelation ($\rho$):** The degree to which a firm's unexplained performance (the error term $\epsilon_{it}$) in one year correlates with its performance in the next year. Highly sticky productivity means a higher $\rho$.
* **Industries ($J$):** The number of discrete industry categories included in the model, used to calculate the degrees of freedom consumed by industry-by-year fixed effects.

### Statistical Standards
* **Alpha ($\alpha$):** The probability of a Type I error (false positive). Set to 0.05 for a standard 95% confidence interval.
* **Power ($1-\beta$):** The probability of correctly detecting a true effect (avoiding a Type II error/false negative). Standard econometric practice sets this at 80% (0.80).

## Output Metrics (The Four KPI Boxes)

* **Clustered Obs ($N$):** The final, total number of firm-year observations required to detect your target MDE. This is the baseline sample size penalized by the Design Effect to account for serial correlation.
* **Balanced Firms ($N_i$):** The required number of unique, individual firms you must track across all $T$ years. Calculated as Clustered Obs divided by the panel horizon ($N / T$).
* **Design Effect:** The mathematical penalty applied to your baseline sample size due to intra-cluster correlation. Calculated as $1 + (T-1)\rho$. If this value is 3.0, you need three times as much data as a pure cross-sectional study.
* **Unclustered $N$:** The theoretical total number of observations required if every single firm-year data point was completely independent (assuming $\rho = 0$).

## Benchmark Thresholds Table Columns

* **Target MDE ($\beta_1$):** Standard baseline effect thresholds evaluated at 1%, 2%, and 5% changes in productivity.
* **Unadjusted Obs ($N$):** The required total observations before adjusting for within-firm serial correlation (DEFF).
* **Clustered Obs ($N_{final}$):** The true required total observations after accounting for both degrees of freedom consumed and the DEFF multiplier.
* **Required Linked Firms ($N_i$):** The specific number of unique firms that must survive the multi-survey linkage process and appear in all years of the balanced panel.

## Interpreting the Results and Curves

The **Required Sample Size Curve Across MDEs** chart plots the Target MDE on the X-axis against the Required Total Observations on the Y-axis. 

* The **solid blue line** represents the true required sample size (Clustered $N$), accounting for serial correlation. 
* The **dashed grey line** represents the theoretical baseline (Unclustered $N$). 
* The gap between these two lines visually demonstrates the "panel penalty"—how much extra data is required simply because longitudinal observations of the same firm are not perfectly independent. 
* The curve decays exponentially: as the effect size you are trying to detect gets smaller (moving left on the X-axis), the required sample size explodes upward.

## Parameter Sensitivity: How Changing Inputs Drives the Results

| Parameter Adjusted | Direction | Impact on Required Sample Size | Econometric Reason |
| :--- | :--- | :--- | :--- |
| **Treatment Share ($p$)** | Move further away from 50% (e.g., down to 10%) | **Increases exponentially** | The variance of the treatment indicator shrinks. A highly imbalanced sample provides less comparative leverage between treated and untreated firms. |
| **Variance Explained ($R^2$)** | Increase | **Decreases linearly** | Better covariates and FEs absorb more background heterogeneity, lowering the residual "noise" floor and making the treatment signal easier to detect. |
| **Panel Horizon ($T$)** | Increase | **Increases slightly** | While tracking firms longer gives you more total observations, the Design Effect penalty ($1 + (T-1)\rho$) inflates simultaneously, meaning those extra years offer diminishing marginal statistical returns. |
| **Autocorrelation ($\rho$)** | Increase | **Increases linearly** | High stickiness means year 2 provides very little *new* information compared to year 1. The model requires more unique firms to compensate for the redundant time-series data. |
| **Alpha / Power** | Stricter (e.g., 99% / 90%) | **Increases heavily** | Demanding higher statistical certainty requires a larger sample to shrink the standard errors further. |