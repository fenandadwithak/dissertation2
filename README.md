# Frequent A&E Attendance in Scotland

## Mapping Emergency Demand: The Deprivation, Demographic, Disease Buden, and Department Patterns Behind Frequent A\&E Attendance in Scotland

> Fenanda Dwitha Kurniasari (S2744048), MSc Statistics with Data Science (Candidate)



---

## Overview

A small group of frequent attenders(people with three or more A&E visits in a year) accounts for
a disproportionate share of demand on Scotland's emergency departments. This project models twelve
months of Public Health Scotland attendance data (rolling year to April 2026) to identify who this
group is and where attendance pressure concentrates, using a negative binomial generalised linear
mixed model (GLMM) with a Health Board random intercept.


---

## Data Sources

All data is publicly available. None is redistributed in this repository, scripts download or expect
local copies of the raw extracts below.

| # | Dataset | Source | Used for |
|---|---------|--------|----------|
| 1 | General Practice Disease Prevalence | [Public Health Scotland](https://publichealthscotland.scot/publications/general-practice-disease-prevalence-data-visualisation/general-practice-disease-prevalence-visualisation-8-july-2025/) | Health Board–level chronic disease prevalence rates (heart failure, depression, etc.) |
| 2 | Monthly A&E Activity — Multiple Attendances | [NHS Scotland Open Data](https://www.opendata.nhs.scot/dataset/monthly-accident-and-emergency-activity-and-waiting-times/resource/7f2e9288-5ea7-4d55-819d-dde4d211c72d) | Attendance counts by Health Board, age, sex, deprivation quintile, and department type |
| 3 | Mid-2025 Population Estimates | [National Records of Scotland](https://www.nrscotland.gov.uk/publications/mid-2025-population-estimates/) | Population offset by Health Board, age band, and sex |
| 4 | Geography Codes and Labels | [NHS Scotland Open Data](https://www.opendata.nhs.scot/dataset/geography-codes-and-labels/resource/652ff726-e676-4a20-abda-435b98dd7bdc) | Health Board code-to-name lookup, used for joins and the choropleth map |

> **Note:** the multiple-attendances extract is refreshed monthly by PHS but each release reports a
> rolling 12-month window, not calendar-month counts. Re-running this pipeline on a newer release will
> shift the reference period accordingly.

---

## Repository Structure

```
.
├── data/                  # Data Input              
│   └── raw/            
├── scripts/               # R scripts / R Markdown
│   └── script.Rmd        
├── figures/               # exported plots (PNG, used in the report)
├── report/                # dissertation PDF 
├── references/            # journal and literatures
└── README.md
```


---

## Methodology Summary

- **Response:** count of frequent attenders (≥3 attendances) per stratum (Health Board × age × sex ×
  deprivation × department type), with `log(population)` as an offset.
- **Model:** negative binomial GLMM (`glmmTMB`), random intercept for Health Board, fitted after a
  variance-to-mean check confirmed substantial overdispersion (Poisson unsuitable).
- **Model selection:** predictors added in four blocks (age/sex → deprivation → department type →
  disease prevalence), each tested via likelihood ratio test and AIC rather than automated search.
- **Sensitivity analysis:** Poisson vs. negative binomial vs. zero-inflated negative binomial (ZINB)
  comparison; DHARMa simulation-based residual diagnostics; an age×sex interaction robustness check.


---

## Requirements

Analysis was run in R. Key packages:

```r
install.packages(c(
  "tidyverse",     # data wrangling and plotting
  "MASS",          # glm.nb()
  "glmmTMB",       # negative binomial GLMM / ZINB extension
  "DHARMa",        # simulation-based residual diagnostics
  "performance"    # icc(), check_collinearity()
))
```

---



## Supervisors

Supervised by Nicole Augustin and Ioannis Papastathopoulos


## Acknowledgements

Public Health Scotland, for sharing domain context on the A&E dataset. See the dissertation's
Acknowledgments and Generative AI Acknowledgement sections for full details.
