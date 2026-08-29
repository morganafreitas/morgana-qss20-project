# How Different Factors Affect Commuting Time in São Paulo Metropolitan Region (RMSP)

This project analyzes the drivers of commute duration in the São Paulo Metropolitan Region (RMSP), Brazil. The project asks how the relative influence of housing/job distribution policy, transportation (transit access) policy, and individual-level individual choice trade-off variables (car ownership and income) compares in explaining commuting time, using a mixed-effects regression with a random intercept for home zone.

## Data Sources 
<b>• METRO Origin-Destination (OD) Survey 2023 ([Banco2023_divulgacao_190225.sav](final_project_data/Banco2023_divulgacao_190225.sav))</b>: Trip-level records, including trip duration, origin/destination.  zone, transport mode, and respondent characteristics. Codebook: [Layout_BD_OD2023_190225.xlsx](final_project_data/Layout_BD_OD2023_190225.xlsx) <br>
<b>• RAIS ([rais_rmsp.csv](final_prokect_data/rais_rmsp.csv), microdados_estabelecimentos, via BigQuery / Base dos Dados)</b>: Job counts by establishment, filtered to RMSP municipalities. <br>
<b>• IBGE 2022 Census ([Agregados_por_setores_basico_BR](final_project_data/Agregados_por_setores_basico_BR.csv))</b>: Household counts by census tract <br>
<b>• Zone/district crosswalks ([Corresp2017_2023_190225.xlsx](final_project_data/Corresp2017_2023_190225.xlsx))</b>: Bridges OD survey zones, RAIS municipalities/districts, and IBGE census tracts to a common zone geography <br>

## Notebooks
• [00_pull_merge](code/00_pull_merge.ipynb) loads the raw input files (RAIS jobs, IBGE census, OD survey, zone crosswalk), builds job_housing_ratio and jobs_accessible_40min, and merges everything into model_df for the analyze notebook. <br>
• [01_analyze](code/01_analyze.ipynb) runs diagnostics on model_df, fits the mixed effects regression, and builds the forest plots and ICC calculations.
