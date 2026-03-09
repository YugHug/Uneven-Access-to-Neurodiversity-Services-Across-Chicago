# Uneven-Access-to-Neurodiversity-Services-Across-Chicago
Neighborhood-level analysis of neurodiversity-related service access in Chicago using OpenStreetMap (Overpass API), hardship indicators, SQL, EDA, and regression models.

## Project Overview
This project examines whether neurodiversity-related child services in Chicago are distributed evenly across neighborhoods, or whether areas facing greater socioeconomic hardship also experience lower measured access to services.

Because OpenStreetMap (OSM) does not directly classify “neurodiversity services,” we construct a proxy measure using healthcare tags and service-name keywords related to speech therapy, occupational therapy, psychology, autism, developmental support, and other child-focused care services.

This is a descriptive analysis (not causal) intended to produce a reproducible pipeline and a shortlist of community areas that may warrant follow-up validation.

## Target Audience
Policy planners, public health/community stakeholders, and organizations interested in neighborhood-level service access and potential gaps.

## Data Sources
1. **OpenStreetMap / Overpass API**: service locations in Chicago (API pull)
2. **Chicago Community Area Boundaries**: used for spatial join to assign services to community areas
3. **Chicago Hardship Index (2008–2012)**: neighborhood socioeconomic indicators (API pull)

## Key Methods
- Overpass API pull → build services dataframe
- Proxy labeling:
  - **Strict proxy**: ND keywords in service name
  - **Broad proxy**: strict keywords OR therapy-related healthcare tags
- GeoPandas point-in-polygon join to assign services → community areas
- Aggregation to community-area service counts
- Normalize by geography using **services per square kilometer**
- Two desert definitions for sensitivity:
  - **desert_zero**: service density == 0
  - **desert_rank25**: bottom 25% density with tie-break on total services
- Modeling:
  - Linear regression predicting service density
  - Logistic regression predicting desert_rank25

## Repository Guide
- `notebooks/Data Analysis Final_Artyom-Jay-Quang.ipynb`  
  Polished narrative notebook (intro, methodology, QA checks, EDA, models, conclusion)

- `sql/Final_SQL file.sql`  
  SQL-only deliverable with commented queries (joins, window functions, group by, subqueries)

- `data/`  
  Main artifacts used by the notebook and SQL:
  - `chicago_osm_services_clean_ndproxy.csv`
  - `services_by_community_area.csv`
  - `community_area_services_plus_hardship_full_with_deserts.csv`
  - `final_project.db`
  - `final_projectV2.db`

- `slides/Uneven-Access-to-Neurodiversity-Services-Across-Chicago.pdf`  
  Final presentation deck

- `appendix/`  
  Raw API outputs and intermediate files (audit trail)

## How to Run
1. Open `notebooks/Data Analysis Final_Artyom-Jay-Quang.ipynb`
2. Run cells top-to-bottom (or load from the CSV/SQLite artifacts in `data/` for faster reproduction)
3. SQL queries can be run against `data/final_project.db`and `final_projectV2.db` 

## Notes / Limitations
- OSM tagging is incomplete and may undercount services.
- Results should be interpreted as a prioritization list for validation, not a definitive inventory.
- Per-child normalization is future work pending a clean under-18 dataset by community area.

## Team
Jay, Quang, Artyom
