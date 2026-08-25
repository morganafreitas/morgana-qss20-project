This project analyzes the drivers of commute duration in the São Paulo Metropolitan Region (RMSP), Brazil. The project asks how the relative influence of housing/job distribution policy, transportation (transit access) policy, and individual-level trade-off variables (e.g. car ownership) compares in explaining commuting time, using a mixed-effects regression with a random intercept for home zone.

**Data Sources** <br>
<ins>METRO Origin-Destination (OD) Survey 2023 (Banco2023_divulgacao_190225.sav)</ins>: Trip-level records, including trip duration, origin/destination.  zone, transport mode, and respondent characteristics. Codebook: Layout_BD_OD2023_190225.xlsx <br>
<ins>RAIS (microdados_estabelecimentos, via BigQuery / Base dos Dados)</ins>: Job counts by establishment, filtered to RMSP municipalities. <br>
<ins>IBGE 2022 Census (Agregados_por_setores_basico_BR)</ins>: Household counts by census tract <br>
<ins>Zone/district crosswalks (correspondencia_zonas.xlsx)</ins>: Bridges OD survey zones, RAIS municipalities/districts, and IBGE census tracts to a common zone geography <br>

**Notebooks**
All analysis is contained in a single notebook: final_project_code.ipynb
