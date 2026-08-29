# How Different Factors Affect Commuting Time in São Paulo Metropolitan Region (RMSP)

This final project for QSS20X26 analyzes the drivers of commute duration in the São Paulo Metropolitan Region (RMSP), Brazil. The project asks how the relative influence of housing/job distribution policy, transportation (transit access) policy, and individual-level individual choice trade-off variables (car ownership and income) compares in explaining commuting time, using a mixed-effects regression with a random intercept for home zone.

# Data  
**METRO Origin-Destination (OD) Survey 2023** ([Banco2023_divulgacao_190225.sav](final_project_data/Banco2023_divulgacao_190225.sav)): Trip-level records, including trip duration, origin/destination.  zone, transport mode, and respondent characteristics. Codebook: [Layout_BD_OD2023_190225.xlsx](final_project_data/Layout_BD_OD2023_190225.xlsx) <br>
**RAIS** ([rais_rmsp.csv](final_prokect_data/rais_rmsp.csv), microdados_estabelecimentos, via BigQuery / Base dos Dados): Job counts by establishment, filtered to RMSP municipalities. <br>
**IBGE 2022 Census** ([Agregados_por_setores_basico_BR](final_project_data/Agregados_por_setores_basico_BR.csv)): Household counts by census tract <br>
**Zone/district crosswalks** ([Corresp2017_2023_190225.xlsx](final_project_data/Corresp2017_2023_190225.xlsx))</b>: Bridges OD survey zones, RAIS municipalities/districts, and IBGE census tracts to a common zone geography <br>

# Order to run
1. [00_pull_merge.ipynb](code/00_pull_merge.ipynb)
- Takes in:
  - RAIS job counts, IBGE census counts, OD survey, and the zone crosswalk (see Data, above)
- What it does:
  - Normalizes municipality/district names (`unidecode`) so RAIS, IBGE, and the zone crosswalk can be joined on text
  - Allocates RAIS jobs and IBGE households down to the OD survey's zone level, then builds `job_housing_ratio` (jobs / households per zone)
  - Builds `jobs_accessible_40min`: for each zone pair with at least 5 weighted OD observations, checks whether the average transit trip is ≤ 40 minutes, then sums destination-zone jobs reachable from each origin zone; converts to `jobs_accessible_per_household`
  - Converts `renda_fa` (monthly household income, R$) to `income_usd` using the 2023 average USD/BRL exchange rate
  - Collapses the raw `modoprin` mode variable into 8 policy-relevant `mode_group` categories, with Car (driver) set as the reference category
  - Merges the zone-level variables onto the trip-level OD data to build `model_df`, drops rows with missing trip duration, and z-scores the continuous predictors (`job_housing_ratio`, `jobs_accessible_per_household`, `qt_auto`, `income_usd`, `idade`)
  - Prints before/after row and NaN counts at each merge step
- Outputs:
  - `final_project_data/interim/model_df.pkl`: dataset used by the analyze notebook

2. [01_analyze.ipynb](code/01_analyze.ipynb)
- Takes in:
  - `final_project_data/interim/model_df.pkl`
- What it does:
  - Runs diagnostics on the predictors: summary stats/histograms, variance inflation factors, and lowess-smoothed scatterplots against trip duration
  - Checks the number of zones and trips per zone (random-intercept grouping variable)
  - Fits a mixed-effects regression (`smf.mixedlm`) of trip duration on the z-scored predictors and categorical controls (sex, trip purpose, mode), with a random intercept per zone
  - Extracts coefficients, confidence intervals, and p-values into a forest-plot-ready table; keeps demographic controls in the fitted model but excludes them from the plotted output
  - Splits results into two forest plots — mode effects, and zone-level/trade-off effects — since Rail transit's large coefficient compresses the scale for the policy-relevant variables
  - Computes conditional and unconditional ICC to assess how much of the variance in commute duration is zone-structured versus individual/trip-level
- Coding notes (see code for more detail):
  - Car (driver) is the mode reference category on both statistical grounds (largest n, stable baseline) and policy grounds (legible transit-vs-auto comparisons)
  - The individual choice trade-off forest plot caption reports each z-scored predictor's actual standard deviation in original units (jobs per household, cars, US$/month) alongside the coefficients, so a "1 SD" effect is interpretable rather than only measurable
  - `job_housing_ratio` did not reach significance (p ≈ 0.1, CI crosses zero), likely reflecting collinearity with `jobs_accessible_per_household` and a low effective sample size for a zone-constant predictor
- Outputs:
  - `transportation_access.png`: forest plot of mode effects (relative to Car (driver))
  - `job_tradeoff.png`: forest plot of zone-level and individual choice trade-off effects (job-housing ratio, jobs accessible, car ownership, household income)
  - Printed ICC values (conditional and unconditional)
 
