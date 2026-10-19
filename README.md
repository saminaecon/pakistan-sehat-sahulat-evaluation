# pakistan-sehat-sahulat-evaluation
An econometric evaluation of Pakistan's Sehat Sahulat Program (SSP) using multi-wave PSLM data (2014–2019) in R, implementing a Triple-Difference (DDD) model with district fixed effects and clustered standard errors
# Evaluating Pakistan's Sehat Sahulat Program (SSP)

## Project Overview
This repository contains an end-to-end econometric pipeline built in **R** to evaluate the impact of Pakistan's **Sehat Sahulat Program (SSP)**—a major social health insurance initiative. Using multi-wave household survey data from the **Pakistan Social and Living Standards Measurement (PSLM)** survey (2014–15 and 2019–20), this project investigates whether the program successfully mitigated the physical distance barrier to healthcare access in Khyber Pakhtunkhwa (KP).

---

## Identification Strategy (Econometric Model)
To measure the causal impact of the insurance scheme, the pipeline implements a **Triple-Difference (DDD)** econometric model:
* **Treatment Group:** Households in KP (where SSP was rolled out).
* **Control Group:** Households in other provinces (where the rollout timeline differed).
* **Pre/Post Periods:** 2014 (Pre-intervention) vs. 2019 (Post-intervention).
* **Intensity/Mechanism:** Distance to the nearest health facility in kilometers.

The empirical specification is estimated using ordinary least squares with fixed effects:
$$\text{DoctorVisit}_{ist} = \beta_1 (\text{Treated}_s \times \text{Post}_t \times \text{Distance}_i) + \text{Controls}_{ist} + \gamma_s + \delta_t + \epsilon_{ist}$$
* $\gamma_s$: District Fixed Effects
* $\delta_t$: Year Fixed Effects
* Standard errors are clustered at the district level.

---

## Data Pipeline & Cleaning Workflow
1. **Multi-Module Data Integration:** Merged household demographic rosters (`sec B` / `roster.dta`), health consultation modules (`Sec D` / `secd.dta`), and geographic distance modules (`Sec G` / `secl.dta`).
2. **Granular Individual Matching:** Joined records using household (`hhcode`) and individual (`idc`) identifiers while handling many-to-many relationship warnings.
3. **Zero-Imputation for Non-Consultations:** Converted missing health records for healthy individuals into zeros (`0 = no doctor visit`) to capture population-level utilization across $>800,000$ cleaned individual observations.

---

## Key Results Summary
* **Sample Size:** $N = 818,940$ individual observations across 191 districts.
* **Controls:** Age and gender controls show strong, statistically significant baseline effects ($p < 0.001$) matching economic intuition (higher healthcare utilization among older individuals and females).
* **DDD Interaction (`treated:post:distance_km`):** Yields a point estimate of `-0.0019` ($p = 0.425$), serving as a rigorous foundational baseline for ongoing robustness checks and asset-index controls.

---

## Repository Structure
* `ssp_pipeline_script.R`: The complete R script containing data loading, cleaning, merging, and regression execution using `haven`, `dplyr`, and `fixest`.
* `ssp_regression_results.txt`: Exported text file containing the final regression output table.
