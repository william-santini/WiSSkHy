# WiSSkHy ![Project Progress](https://img.shields.io/badge/Progress-60%25-brightgreen)
Wiki Soft Suite for Hydrology   
Distill your data. Hydrate your science...
![Views](https://komarev.com/ghpvc/?username=williamsantini&label=Views&color=0e75b6&style=flat)


## The WiSSkHy framework
**`WiSSkHy` is a local, modular, and fully open-source** framework built with `R Shiny`, `Julia`, and `SQLite` (with migration planned to `PostgreSQL`) for hydrological data management and visualization.. Its database architecture is conceptually aligned with the [OGC SensorThings API standard](https://docs.ogc.org/is/18-088/18-088.html), and designed following the [FAIR principles](https://www.go-fair.org/fair-principles/) (*Findable, Accessible, Interoperable, Reusable*), with a focus on interoperability (OGC/ISO standards) and reusability (open formats, reproducible workflows).
   
See the live demo here: https://sno-hybam.shinyapps.io/WiSSkHy/
   
<img width="1870" height="979" alt="image" src="https://github.com/user-attachments/assets/71a519ba-cd3c-4bc6-a2b6-af1a4500ca9c" />
   
<img width="1898" height="900" alt="image" src="https://github.com/user-attachments/assets/5010300b-2bd3-4398-b348-465cdcb25ba9" />

   
It provides a complete workflow to support hydrological services and researchers, including:
   
- Data import & storage in a local SQLite database
- Database management (visualize, add, or delete tables and fields)
- Data cleaning
- Gap filling and consistency analysis
- Data wrangling
- Data visualization & exploration (interactive plots and tables)
- Advanced data processing and analysis with a suite of tools designed for both **operational use and research**, including:
  - ADCP measurements
  - Rating curves
  - Sediment analysis (solid gauging, particle size distribution, etc.)
  - Flux calculation
  - Remote sensing data processing
  - Geochemistry
- Data export to `.csv`, `.xlsx`, and other formats
- Automated report generation in LaTeX, Word, and HTML (using R Markdown and Quarto)

The `WiSSkHy-App` was created to provide access to the main tools of the `WiSSkHy` environment.  
Beyond these core features, users can:
- Add new tools directly in R-Shiny, either within the existing apps or independently.
- Write custom scripts in R, Python, or other languages to work with the WiSSkHy SQLite database.
- Use the apps to automatically generate SQL queries that can be reused in custom code/script.





> [!NOTE]  
> `WiSSkHy` not only provides access to core tools but also acts as a collaborative "Wiki":  
> users can contribute, share, and reuse tools or scripts within the community.  
> This flexibility encourages collective development, knowledge exchange, and the integration of custom processing, analysis, or visualization workflows, all while leveraging the common `WiSSkHy` database structure.

![Schéma](https://github.com/user-attachments/assets/03230a04-b6dd-41fa-8070-0fb65640880b)

## Collaborative use of the database `WiSSkHy-db`

SQLite supports multi-user access, but with important limitations:  
- **Read operations** can be performed simultaneously by multiple users.  
- **Write operations** are exclusive: only one user can modify the database at a time, while others must wait until the operation is completed.  

To facilitate collaboration, WiSSkHy includes a **"Refresh Database"** button so that users can always reload the most recent changes made by others.  

`WiSSkHy` is primarily designed as a **local tool**, complementing centralized server solutions. It allows researchers to fully leverage the power of **R, Python**, and other tools for data curation, analysis, and scientific research.  

> [!TIP]  
> **Future development**  
> A **PostgreSQL** version of WiSSkHy is planned, enabling a true multi-user environment with efficient concurrency management.  
> This version will follow all the FAIR principles (Findable, Accessible, Interoperable, Reusable).


## Project status

`WiSSkHy` is actively under development. The current progress is estimated at **60% complete**.

### Features already implemented
- Core R-Shiny Apps for database management, visualization, and analysis
- Data import/export and quality control tools
- Automated report generation in LaTeX and HTML
- Support for multiple parameter types and temporal resolutions
- ADCP tools
- Rating tools

### Planned features
- Full multi-user support with a PostgreSQL backend
- Additional analysis modules and integration with external tools
- Expanded collaborative features for sharing scripts and tools within the community

> [!TIP]  
> `WiSSkHy` continues to evolve as a collaborative tool for hydrological data management, aiming to leverage the power of R, Python, and other languages while ensuring reproducible and flexible workflows.




# Getting started with the `WiSSkHy-App`

## Installation
You can either run the Shiny apps with RStudio or use the WiSSkHy.exe. 
If you need to access the WiSSkHy SQLite database externally, you can install DBeaver or SQLite Browser.

## Pipeline database

### Start by creating or loading a `WiSSkHy-db`

### Thing (station or platform)

### Time series

### Configuration tables

### Main tables

### Other tables

### Edit field



## Pipeline Discrete Measurements


## Pipeline Time Series
This module manages the full time series/data workflow, from raw import to formatted exports. It ensures traceability, quality control, and reproducibility of hydrological time series.

### Pipeline overview:
1. Import             → Load raw datasets from local files, sensors, or databases
2. Clean & Export     → Detect, flag, and correct anomalies (missing values, duplicates, outliers)
3. Wrangle            → Reshape, merge, and harmonize datasets (units, parameters, metadata)
4. Fill & Validate    → Fill data gaps using statistical, mechanistic, or ML estimators; perform inter-station consistency checks
5. Analyze            → Generate exploratory statistics, diagnostics, and performance indicators
6. Summarize & Report → Produce automatic reports (RMarkdown / HTML / PDF) with interpretation
7. Format & Export    → Export datasets in standardized formats (ANA, OTCA, WMO, WaterML2, etc.)

This sequential workflow guarantees high-quality, standardized, and shareable data products.

### Clean & Export

The **Clean & Export** module provides tools to prepare and export validated time series from the WiSSkHy database.  
It allows users to filter, clean, and format hydrological and sediment data prior to analysis or sharing.

#### Main features
- Import and preview of time series from the SQLite database.  
- Selection and filtering by station, parameter, and time period.  
- Detection and removal of missing values, duplicates, and **replicates** (multiple measurements for the same date).  
- Identification and treatment of **zero, negative, or unrealistic values** according to parameter-specific rules.  
- Detection of **repeated patterns, shifts, and anomalies** using time-series motif analysis.  
- Application of quality flags, correction rules, and statistical summaries.  
- Export of cleaned datasets in `.xlsx` or `.csv` formats, with complete metadata and traceability.  

#### Main R packages
- Data manipulation: `dplyr`, `tidyr`, `data.table`, `lubridate`  
- Visualization: `ggplot2`, `plotly`  
- Pattern detection: `TSMP`, `MatrixProfile`  
- File export: `openxlsx`, `readr`
  
### Fill & Validate 

_(WiSSkHy-CoFi: Consistency & Filling — part of the WiSSkHy data toolbox)_

Tools for **time-series consistency analysis** and **gap filling** in hydrological and sediment datasets.  
The module builds, tests, and validates estimators (statistical, mechanistic, or machine learning) before any automated filling.  
It also supports **analyst-in-the-loop** workflows for interactive review and correction.

---

## Methods

### Statistical
- Simple, multivariate, and multiple regressions (`MASS::rlm`)
- Statistical propagation between stations (correlation-based)
- Seasonal climatologies (daily/monthly averages)
- ARIMA / SARIMA models for short- and seasonal predictions (`forecast`)

### Hydraulic / Hydrological
- Simplified flow routing (Muskingum, Muskingum–Cunge, Kinematic Wave)
- Optional floodplain attenuation and reach parameters

### Machine Learning / AI
- Classical ML via `tidymodels`, `stacks` (GLM, RF, GBM, XGBoost)
- Deep learning (`keras`, `torch`): LSTM / GRU for temporal sequences

### Analyst-in-the-Loop
- Semi-automatic estimator calibration and correction
- Forecasting inspired by **Prophet** (Taylor & Letham, 2018)
- Human-supervised validation before applying gap filling

---

### Workflow

1. **Clean & align** receiver and donor time series  
2. **Build** a unified data frame (`df_init`, daily or sub-daily with matchups)  
3. **Select** training periods and apply soft masks (`usefit_*`)  
4. **Train** estimators → compute diagnostics → store residuals  
5. *(Later)* Apply estimators for controlled gap filling and consistency analysis  

---

### Core R Dependencies

| Category | Packages |
|-----------|-----------|
| Data handling | `dplyr`, `tidyr`, `lubridate`, `readxl`, `data.table` |
| Modeling | `MASS`, `broom`, `forecast`, `tidymodels`, `stacks` |
| ML / Deep learning | `keras`, `torch` (optional) |
| Visualization | `plotly` |
| Parallel | `doParallel` |

<img width="1536" height="791" alt="newplot" src="https://github.com/user-attachments/assets/0512d134-cc83-4add-a667-6001b9dc240e" />

### Wrangle



### Summarize & Report


### Format & Export


## Pipeline Calibrate
This module provides tools for building, fitting, and validating stage–discharge relationships (rating curves) and related calibration datasets. 
It complements the Time Series and Compute modules by allowing users to derive, manage, and evaluate empirical or model-based calibration equations.

This module ensures consistency and reproducibility of discharge calibration processes within the WiSSkHy environment.

### Pipeline overview:
- Rating Tools → Fit and visualize stage–discharge (Q–H) relationships using power laws, regressions, or robust estimators; manage input points and extrapolation.
- Diagnostics → Assess calibration performance through residual plots, error metrics, temporal validity, and shift detection.
- Uncertainty → Quantify uncertainty and confidence intervals of rating curves and computed discharges; compare against ADCP or reference measurements.

## Pipeline Compute

This module provides tools for computing water discharge, material fluxes (loads), and hydrological indicators from validated time series and calibration datasets.
It complements the Time Series and Calibrate modules by enabling the quantitative derivation of hydrological, sedimentary, and water-quality metrics.

### Pipeline overview:
Water Discharges  → Compute discharge (Q) from stage (H) using rating curves or hydraulic relationships; manage extrapolation, shifts, and versioning.
Fluxes            → Estimate material fluxes or loads (e.g., sediment, nutrients, carbon)
    #                         from discharge and concentration data using multiple methods:
    #                         direct integration, interpolation, regression, or flow-weighted means.
    #  3. Mass Balances     → Derive water and sediment budgets over defined periods or basins;
    #                         compute cumulative inflows/outflows and storage changes.
    #  4. Flow Indicators   → Calculate hydrological indices such as specific discharge (Qs),
    #                         runoff coefficient, baseflow index, flashiness, and seasonal metrics.
    #  5. Balance Reports   → Summarize and visualize computed discharges, fluxes, and balances;
    #                         generate automatic reports or export summary tables.
    #
    # This module transforms calibrated and validated observations into discharge, flux,
    # and balance metrics suitable for scientific analysis, modeling, and reporting within WiSSkHy.

## Pipeline Advanced Tools

### Pipeline overview:

This module provides advanced hydrological, sedimentary, and geochemical analysis tools. It complements the Data pipeline by offering computation, modeling, visualization, and reporting utilities for calibrated, derived, or cross-referenced datasets.

1. Calibrate     → Define and fit rating curves; establish stage–discharge relationships
2. Compute       → Derive discharges, sediment fluxes, and flow-normalized indicators
3. Model         → Apply hydraulic, hydrological, or statistical models
4. Remote Sense  → Process and visualize altimetry and water colour (optical) observations
5. Geochemistry  → Perform statistical analyses, compute ionic ratios, and generate diagrams (Piper, Stiff, Schoeller, Gibbs); compare with standards or thresholds
6. ADCP Advanced → Use HydSed-ADCP tools for velocity profiling, shear velocity (u*), concentration fitting, and moving-bed analyses
7. Compare       → Cross-check datasets across sources, stations, or observation types
8. Report        → Generate automatic reports (Concentration Analysis, Field Report, etc.)

This toolbox enables expert-level interpretation, modeling, and cross-domain integration of hydrological, sedimentary, and geochemical datasets within WiSSkHy.


### ADCP Advanced



<img width="1886" height="862" alt="image" src="https://github.com/user-attachments/assets/1665a642-bc83-4fca-9146-47849f296ebf" />










## Query generator Tab



## Database Structure

`WiSSkHy` manages **stations** as operational entities (hydrological sites equipped with sensors or used for field campaigns) and links them to **features of interest (FOI)**, i.e. the real-world entities being observed (rivers, cross-sections, basins, lakes, transects...).  

The database stores both:
- **Direct observations** coming from sensors (e.g., limnigraphs, ADCP), and  
- **Derived data** (e.g., discharge, sediment fluxes), which are not produced by a physical sensor but calculated from raw data.  

Because of this, `WiSSkHy` relies on the concept of **time series** rather than direct **datastreams** as defined in the SensorThings standard. However, each time series can still be associated with a sensor when relevant.  

The database structure makes it possible to map `WiSSkHy` entities (things, time series, data, FOI) to the [OGC SensorThings API standard](https://docs.ogc.org/is/18-088/18-088.html). `WiSSkHy` can therefore be considered as **conceptually aligned with the SensorThings model**, while remaining flexible enough to handle both raw and derived hydrological data.  

**UUIDs** are automatically assigned to each core entity (`things`, `feature_of_interest`, `time_series`, `data`, `discrete_sampling`, etc.) to guarantee uniqueness and to facilitate merging of databases from different sources without conflicts. The UUID is generated directly in the SQL code and the user don't have to care about it.

Discrete measurements (punctual or spot samples) are also handled as time series, since each measurement is associated with a date.

> [!Important]
> A **time series (ts)** belongs to a **station** and is defined as the combination of a **parameter** (Observed Property) and a **temporal record**:  
> [time series] = [parameter] + [name] + [temporal resolution],
> where:  
> [temporal record] = [name] + [temporal resolution].

For instance, a same station can have many water disharge time series **Q** with different temporal resolutions and contexts (names):  
- **Q_I_raw** → Water Discharge (Q) Instantaneous (I) Raw data (raw),
- **Q_D_clean** → Water Discharge (Q) Daily (D) QA/QC data (clean)
- **Q_I_raw-XXX** → Water Discharge (Q) Instantaneous (I) Raw data managed by another institution XXX (e.g., NGO, hydrological service, electricity company...) (raw-XXX).

When the user creates a station, he must choose and configure the attached time series, with parameter, temporal resolution, and name.

---



## Database Views

The `WiSSkHy` database uses incremental IDs as primary keys instead of composite keys in order to optimize SQL joins and group operations.  
To provide more comprehensive and user-friendly access to the data, WiSSkHy defines a set of **SQL views**.

These views:  
- Expose meaningful relationships between tables without requiring complex joins.  
- Combine metadata (e.g. station, platform, parameter) with observation values.  
- Facilitate queries for analysis, reporting, and data exchange.  
- Are especially convenient for **R-Shiny applications**, since the app can directly query a ready-to-use view instead of building and maintaining complex joins in the server code.  
- Some views are equipped with **INSTEAD OF triggers**, so they behave like editable tables (`INSERT` / `UPDATE` / `DELETE`).  


### View `vw_station`

Provides a **station-centric view** combining `thing` and its most recent `location`.

| Id_Station | Station_Name | Lon    | Lat    | Elevation | Country | Basin_Large | Basin    | Basin_Small | Administrator |
|------------|--------------|--------|--------|-----------|---------|-------------|----------|-------------|---------------|
| LAGARTO    | Lagarto      | -73.45 | -10.77 | 150       | Peru    | Amazon      | Ucayali  | Tamaya      | SENAMHI       |
| IQUITOS    | Iquitos      | -73.25 |  -3.75 | 104       | Peru    | Amazon      | Amazonas | Nanay       | ANA           |


### View `v_sed_sampling`

Provides a **unified view of sediment samples**, combining metadata from `sampling`, `sampling_event`, `sample_sed`, and associated `data`.


| sampling_type       | station  | ts_name_code | origin | sensor       | date_time           | methode | protocol | no_vertical | no_point | no_replicate | lon    | lat    | depth | total_depth | sample_comment     | params_json                       |
|---------------------|----------|--------------|--------|--------------|---------------------|---------|----------|-------------|----------|--------------|--------|--------|-------|-------------|-------------------|-----------------------------------|
| sediment.grab       | LAGARTO  | RAW          | SM     | StaffGauge   | 2025-06-10 10:30:00 | ISO-748 | Grab     | 1           | 1        | 1            | -73.45 | -10.77 | 2.5   | 5           | High flow sampling | {"Q":1420.5,"H":3.45,"Ctot":220} |
| sediment.integrated | IQUITOS  | RAW          | DS     | ADCP Ucayali | 2025-06-11 11:00:00 | ISO-4363| Vertical | 1           | 1        | 1            | -73.25 |  -3.75 | 3.0   | 6           | Integrated sample  | {"Q":980.2,"H":2.87,"Ctot":180}  |



### View `vw_data_full`

Provides a **complete view of observations (`data`)** enriched with metadata from related tables:  
- Time series (`time_series`, `ts_parameter`, `ts_name`, `ts_resolution`)  
- Station/platform (`thing`, `location`)  
- Quality flags (`quality`)  
- Sensor and origin (`sensor`, `data_origin`)  

This view makes it easier to query validated data with all metadata in one place.


| id  | uuid                                 | date_time           | value  | quality | ts_code        | parameter | unit | resolution | station  | lat    | lon    | sensor       | origin | description                        |
|-----|--------------------------------------|---------------------|--------|---------|----------------|-----------|------|------------|----------|--------|--------|--------------|--------|------------------------------------|
| 1   | 550e8400-e29b-41d4-a716-446655440600 | 2025-01-01 00:00:00 | 1420.5 | ok      | LAGARTO|Q|D  | Discharge | m³/s | Daily      | LAGARTO  | -10.77 | -73.45 | Pressure #1  | PS     | Daily discharge at Lagarto         |
| 2   | 550e8400-e29b-41d4-a716-446655440601 | 2025-01-02 00:00:00 | 1387.3 | ok      | LAGARTO|Q|D  | Discharge | m³/s | Daily      | LAGARTO  | -10.77 | -73.45 | Pressure #1  | PS     | Daily discharge at Lagarto         |
| 3   | 550e8400-e29b-41d4-a716-446655440602 | 2025-01-01 12:00:00 |   3.45 | DBT     | LAGARTO|H|I  | Water lvl | m    | Instant.   | LAGARTO  | -10.77 | -73.45 | Radar #1     | PS     | Instantaneous water level (raw)    |
| 4   | 550e8400-e29b-41d4-a716-446655440603 | 2015-07-01 00:00:00 | 220.0  | P       | IQUITOS|Ctot|M | Sed. conc | mg/L | Monthly    | IQUITOS  |  -3.75 | -73.25 | Derived      | R      | Reconstructed sediment conc. value |

➡️ **Usage**:  
Instead of joining 6–7 tables manually, users can query directly:  
```sql
SELECT date_time, value, parameter, unit, station, quality
FROM vw_data_full
WHERE station = 'LAGARTO' AND parameter = 'Discharge'
  AND date_time BETWEEN '2025-01-01' AND '2025-01-31';
```



## Configuration tables

It is possible to easily configure the database using the **configuration tables**. These tables are modifiable if needed and ensure the **homogeneity** of the database.

### Table `thing_type`

Defines the different types of "things", **stations or platforms**, each identified by a unique short code.

| id | type     | code | name           | description                                  |
|----|----------|------|----------------|----------------------------------------------|
| 1  | station  | H    | Hydrological   | Station measuring discharge and water level  |
| 2  | station  | HV   | Hydrological   | Virtual station measuring discharge and water level |
| 3  | station  | M    | Meteorological | Station measuring rainfall, wind, etc.       |
| 4  | platform | V    | Vessel         | Mobile platform (e.g., ADCP vessel)          |
| 5  | platform | D    | Drone          | Aerial platform for measurements             |


### Table `foi_type`

Reference table that defines the different **types of Feature of Interest (FoI)** used in observations.  
Each type has a unique identifier, a UUID, and a descriptive label.

| id | uuid                                 | name              | description                                  |
|----|--------------------------------------|-------------------|----------------------------------------------|
| 1  | 550e8400-e29b-41d4-a716-446655440040 | River reach       | A section of a river monitored for discharge |
| 2  | 550e8400-e29b-41d4-a716-446655440041 | Lake              | A natural or artificial lake                 |
| 3  | 550e8400-e29b-41d4-a716-446655440042 | Catchment area    | Drainage basin associated with a station     |
| 4  | 550e8400-e29b-41d4-a716-446655440043 | Monitoring well   | Groundwater observation point                |


### Table `ts_parameter` (ObservedProperty)

Reference table that defines the **parameters** of time series (e.g., discharge, water level, sediment concentration).  
Each parameter has a unique `uuid`, `code`, and is associated with a measurement unit.  
Users can also define additional custom parameters if needed.

| id | uuid                                 | code  | name                        | unit  | description                               |
|----|--------------------------------------|-------|-----------------------------|-------|-------------------------------------------|
| 1  | 550e8400-e29b-41d4-a716-446655440060 | Q     | Discharge                   | m³/s  | River discharge                           |
| 2  | 550e8400-e29b-41d4-a716-446655440061 | H     | Water level                 | m     | Water stage / river height                |
| 3  | 550e8400-e29b-41d4-a716-446655440062 | T     | Water temperature           | °C    | In-situ water temperature                 |
| 4  | 550e8400-e29b-41d4-a716-446655440063 | Ctot  | Total sediment concentration| mg/L  | Total suspended sediment concentration    |
| 5  | 550e8400-e29b-41d4-a716-446655440064 | Cs    | Sand concentration          | mg/L  | Fraction of suspended sand in suspension  |


Default codes in WiSSkHy include for example:  
- **Q** = Water discharge  
- **H** = Water level  
- **Ctot** = Total Suspended Sediment Concentration  
- **Cs** = Sand concentration  

Users may extend this list by adding new parameters as required.


### Table `ts_code`

Reference table that defines **names for time series** (e.g., raw data, corrected data, simulated data).  
A combination of `code` and `category` must be unique, allowing reuse of codes across different categories.

| id | uuid                                 | code   | name                 | category   | description                                |
|----|--------------------------------------|--------|----------------------|------------|--------------------------------------------|
| 1  | 550e8400-e29b-41d4-a716-446655440070 | RAW    | Raw data             | observed   | Original unprocessed data                   |
| 2  | 550e8400-e29b-41d4-a716-446655440071 | CORR   | Corrected data       | observed   | Data corrected after quality control        |
| 3  | 550e8400-e29b-41d4-a716-446655440072 | SIM    | Simulated discharge  | modelled   | Modelled discharge from SWAT                |
| 4  | 550e8400-e29b-41d4-a716-446655440073 | RECON  | Reconstructed series | derived    | Gap-filled or statistically reconstructed   |


### Table `ts_resolution`

Defines the **default time resolutions** used for time series.  
Each resolution has a unique `code`.

| id | uuid                                 | code | label          | description                                                                 |
|----|--------------------------------------|------|----------------|-----------------------------------------------------------------------------|
| 1  | 550e8400-e29b-41d4-a716-446655440080 | I    | Instantaneous  | Raw values without temporal aggregation (as recorded, possibly irregular).  |
| 2  | 550e8400-e29b-41d4-a716-446655440081 | ss   | Second         | Values recorded or aggregated at a one-second resolution.                   |
| 3  | 550e8400-e29b-41d4-a716-446655440082 | mm   | Minute         | Values recorded or aggregated at a one-minute resolution.                   |
| 4  | 550e8400-e29b-41d4-a716-446655440083 | hh   | Hour           | Values recorded or aggregated at an hourly resolution.                      |
| 5  | 550e8400-e29b-41d4-a716-446655440084 | D    | Daily          | Aggregated over 24 hours (mean, max, min, or sum depending on the variable).|
| 6  | 550e8400-e29b-41d4-a716-446655440085 | M    | Monthly        | Aggregated over a calendar month.                                           |
| 7  | 550e8400-e29b-41d4-a716-446655440086 | Y    | Yearly         | Aggregated over a calendar or hydrological year.                            |


### Table `sensor_family`

Reference table that defines **sensor families** (general categories of instruments).  
Each sensor belongs to exactly one family.

| id | uuid                                 | name       | description                          |
|----|--------------------------------------|------------|--------------------------------------|
| 1  | 550e8400-e29b-41d4-a716-446655440400 | Pressure   | Pressure-based water level sensors   |
| 2  | 550e8400-e29b-41d4-a716-446655440401 | Radar      | Radar water level sensors            |
| 3  | 550e8400-e29b-41d4-a716-446655440402 | ADCP       | Acoustic Doppler Current Profilers   |


### Table `sensor`

List of instruments used to collect data.  
Each sensor is linked to a `sensor_family`, a `data_origin`, and a `ts_parameter`.  
Examples include staff gauges, pressure/radar sensors, or ADCP devices.

| id | uuid                                 | name         | type     | manufacturer | model   | serial_number | sensor_family_id | origin_id | ts_parameter_id | date_install | date_remove | description                     |
|----|--------------------------------------|--------------|----------|--------------|---------|---------------|------------------|-----------|-----------------|--------------|-------------|---------------------------------|
| 1  | 550e8400-e29b-41d4-a716-446655440410 | Pressure #1  | Pressure | OTT          | PLS-C   | 12345         | 1                | 1         | 2 (Water level) | 2010-01-01   |             | Main pressure sensor at station |
| 2  | 550e8400-e29b-41d4-a716-446655440411 | Radar #1     | Radar    | Kongsberg    | RWL-200 | 98765         | 2                | 1         | 2 (Water level) | 2016-06-10   |             | Replaced pressure sensor        |
| 3  | 550e8400-e29b-41d4-a716-446655440412 | ADCP Ucayali | ADCP     | Teledyne     | RioPro  | A5678         | 3                | 2         | 1 (Discharge)   | 2018-11-05   | 2018-11-20  | ADCP campaign on Ucayali River  |


### Table `data_origin`

Reference table that defines the **origin of data**.  
It groups families of sensors or methods and makes it easy to identify the provenance of an observation or dataset.  
Each record has a unique `code` and a descriptive `name`.

| id | uuid                                 | code | name                   | description                             | date_start | date_end |
|----|--------------------------------------|------|------------------------|-----------------------------------------|------------|----------|
| 1  | 550e8400-e29b-41d4-a716-446655440500 | SM   | Staff Measurement      | Manual field measurements                | 2000-01-01 |          |
| 2  | 550e8400-e29b-41d4-a716-446655440501 | PS   | Physical Sensor        | Automated in-situ sensors (radar, pressure, etc.) | 2010-01-01 |          |
| 3  | 550e8400-e29b-41d4-a716-446655440502 | WG   | Water Gauging          | Discharge measurement campaigns          |            |          |
| 4  | 550e8400-e29b-41d4-a716-446655440503 | RS   | Remote Sensing         | Satellite or drone-based measurements    | 2015-01-01 |          |
| 5  | 550e8400-e29b-41d4-a716-446655440504 | M    | Modelling              | Outputs from hydrological models         |            |          |
| 6  | 550e8400-e29b-41d4-a716-446655440505 | RSM  | RS + Modelling         | Combination of RS and modelling products |            |          |
| 7  | 550e8400-e29b-41d4-a716-446655440506 | R    | Reconstructed          | Gap-filled or reconstructed data         |            |          |
| 8  | 550e8400-e29b-41d4-a716-446655440507 | INT  | Interpolated           | Values obtained by interpolation         |            |          |

➡️ **Usage**:  
- Linked in the `sensor.origin_id` field.  
- Users may add additional custom `origin` codes if needed.


### Table `quality`

Reference table that defines the **quality flags** used for observations in `data`.  
It can be customized by each institute to specify standardized codes and labels that are applied consistently across datasets.  
Each record has a short `code`, a `label`, and an optional description.

| id | uuid                                 | code | label       | description                                   |
|----|--------------------------------------|------|-------------|-----------------------------------------------|
| 1  | 550e8400-e29b-41d4-a716-446655440200 | ok   | OK          | Valid / reliable observation                  |
| 2  | 550e8400-e29b-41d4-a716-446655440201 | DBT  | Doubtful    | Suspect or uncertain value                    |
| 3  | 550e8400-e29b-41d4-a716-446655440202 | P    | Partial     | Incomplete measurement or partially valid     |
| 4  | 550e8400-e29b-41d4-a716-446655440203 | CUM  | Cumulative  | Value represents an accumulation over a period |

➡️ **Usage**:  
- Linked in the `data.quality_id` field.  
- Ensures consistent handling of data validation and uncertainty.



---

## Main tables


### Table `thing`

Stores the list of **stations and platforms** (the actual "things") with their identifiers, attributes, and metadata.  
Each `thing` is linked to a `thing_type`.

| id | uuid                                 | code   | name                | thing_type_id | basin_large | basin       | river   | lake | country | is_mobile | date_start | date_end | description                  |
|----|--------------------------------------|--------|---------------------|---------------|-------------|-------------|---------|------|---------|-----------|------------|----------|------------------------------|
| 1  | 550e8400-e29b-41d4-a716-446655440000 | ST001  | Lagarto Station     | 1             | Amazon      | Ucayali     | Ucayali |      | Peru    | FALSE     | 2009-01-01 |          | Main hydrological station    |
| 2  | 550e8400-e29b-41d4-a716-446655440001 | ST002  | Atalaya Met Station | 3             | Amazon      | Ucayali     | Tambo   |      | Peru    | FALSE     | 2012-06-15 |          | Meteorological observations  |
| 3  | 550e8400-e29b-41d4-a716-446655440002 | PL001  | ADCP Boat           | 4             | Amazon      | Ucayali     | Ucayali |      | Peru    | TRUE      | 2018-11-05 |          | Mobile ADCP measurement unit |
| 4  | 550e8400-e29b-41d4-a716-446655440003 | DR001  | Survey Drone        | 5             | Amazon      | Marañón     | Marañón |      | Peru    | TRUE      | 2020-03-10 |          | Drone for aerial surveys     |


### Table `location`

Stores the **current geographical location** of each `thing` (station or platform).  
Each `thing` can have only **one current location** (1-to-1 relation).

| id | uuid                                 | thing_id | name             | latitude  | longitude | elevation | location_geojson                                       | description              |
|----|--------------------------------------|----------|------------------|-----------|-----------|-----------|--------------------------------------------------------|--------------------------|
| 1  | 550e8400-e29b-41d4-a716-446655440010 | 1        | Lagarto Station  | -10.7321  | -73.7652  | 230       | `{"type":"Point","coordinates":[-73.7652,-10.7321]}`   | Main hydrological station |
| 2  | 550e8400-e29b-41d4-a716-446655440011 | 2        | Atalaya Met      | -10.7200  | -73.7500  | 240       | `{"type":"Point","coordinates":[-73.7500,-10.7200]}`   | Meteorological station   |
| 3  | 550e8400-e29b-41d4-a716-446655440012 | 3        | ADCP Boat        | -10.7100  | -73.7700  |           | `{"type":"Point","coordinates":[-73.7700,-10.7100]}`   | Mobile ADCP unit         |
| 4  | 550e8400-e29b-41d4-a716-446655440013 | 4        | Survey Drone     | -10.7000  | -73.7600  |           | `{"type":"Point","coordinates":[-73.7600,-10.7000]}`   | Drone for aerial surveys |


### Table `historical_location`

Stores the **past geographical locations** of each `thing` (station or platform).  
Unlike the `location` table (current position), this one allows tracking **changes of position over time**.

| id | uuid                                 | thing_id | name               | location_geojson                                      | time_start  | time_end    | description                 |
|----|--------------------------------------|----------|--------------------|-------------------------------------------------------|-------------|-------------|-----------------------------|
| 1  | 550e8400-e29b-41d4-a716-446655440020 | 1        | Lagarto Station    | `{"type":"Point","coordinates":[-73.7700,-10.7350]}`  | 2005-01-01  | 2008-12-31  | Initial station location    |
| 2  | 550e8400-e29b-41d4-a716-446655440021 | 1        | Lagarto Station    | `{"type":"Point","coordinates":[-73.7652,-10.7321]}`  | 2009-01-01  |             | Current station location    |
| 3  | 550e8400-e29b-41d4-a716-446655440022 | 3        | ADCP Boat Mission  | `{"type":"LineString","coordinates":[[...],[...]]}`   | 2018-11-05  | 2018-11-20  | Mobile survey track (ADCP)  |
| 4  | 550e8400-e29b-41d4-a716-446655440023 | 4        | Drone Flight Zone  | `{"type":"Polygon","coordinates":[[...]]}`            | 2020-03-10  | 2020-03-10  | Drone survey campaign       |


### Table `observer`

Stores the **people or institutions** responsible for making observations, with their contact information.

| id | uuid                                 | obs_name          | obs_address               | obs_mail                 | obs_phone     | country | obs_institute        | comment                   |
|----|--------------------------------------|-------------------|---------------------------|--------------------------|---------------|---------|----------------------|---------------------------|
| 1  | 550e8400-e29b-41d4-a716-446655440030 | Jhonatan Perez    | Jr. Amazonas 123, Lima    | jperez@example.com       | +51 987654321 | Peru    | SENAMHI              | Field hydrologist         |
| 2  | 550e8400-e29b-41d4-a716-446655440031 | Nilton Fuertes    | Calle Ucayali 45, Iquitos | nfuertes@example.com     | +51 912345678 | Peru    | ANA                  | Data validation officer   |
| 3  | 550e8400-e29b-41d4-a716-446655440032 | Maria Gonzalez    | Av. Amazonia 99, Manaus   | mgonzalez@example.org    | +55 929999888 | Brazil  | INPA                 | Remote sensing specialist |
| 4  | 550e8400-e29b-41d4-a716-446655440033 | Hydrology Dept.   | Washington DC             | hydrodept@example.org    | +1 2025550101 | USA     | USGS                 | Institutional observer    |


### Table `thing_observer`

Defines the **many-to-many relationship** between `things` (stations or platforms) and `observers` (people or institutions).  
This allows tracking which observer was responsible for a given `thing`, and during which period.

| thing_id | observer_id | valid_from | valid_to   |
|----------|-------------|------------|------------|
| 1        | 1           | 2009-01-01 | 2015-12-31 |  
| 1        | 2           | 2016-01-01 |            |  
| 2        | 3           | 2012-06-15 |            |  
| 3        | 4           | 2018-11-05 | 2018-11-20 |  

➡️ Example use cases:  
- **Lagarto Station (thing_id=1)** was first managed by observer 1, then by observer 2.  
- **ADCP Boat (thing_id=3)** was associated with observer 4 only for the duration of a survey mission.  


### Table `feature_of_interest`

Stores the different **Features of Interest (FoI)** such as rivers, lakes, or catchments that are linked to observations.  
Each FoI is uniquely identified by a `uuid` and `code`, and classified by a `foi_type`.

| id | uuid                                 | code   | name              | foi_type_id | basin_area | geom_geojson                                        | valid_from | valid_to | description                        |
|----|--------------------------------------|--------|-------------------|-------------|------------|----------------------------------------------------|------------|----------|------------------------------------|
| 1  | 550e8400-e29b-41d4-a716-446655440050 | RIV001 | Ucayali River     | 1           |            | `{"type":"LineString","coordinates":[[...],[...]]}` | 2000-01-01 |          | River reach for discharge monitoring |
| 2  | 550e8400-e29b-41d4-a716-446655440051 | LAK001 | Lake Titicaca     | 2           | 85600      | `{"type":"Polygon","coordinates":[[...]]}`          |            |          | Transboundary natural lake          |
| 3  | 550e8400-e29b-41d4-a716-446655440052 | CAT001 | Atalaya Catchment | 3           | 152000     | `{"type":"Polygon","coordinates":[[...]]}`          | 2010-01-01 |          | Drainage basin upstream of Atalaya  |
| 4  | 550e8400-e29b-41d4-a716-446655440053 | GW001  | Well-23           | 4           |            | `{"type":"Point","coordinates":[-72.3,-10.7]}`      | 2018-05-01 |          | Groundwater observation well        |

### Table `time_series`

Stores the definition of a **time series**.  
Each time series is uniquely identified by a `uuid` and `code`, and is linked to:  
- a `thing` (station or platform)  
- a `feature_of_interest` (river reach, catchment, well, etc.)  
- a `ts_parameter` (e.g., discharge, water level)  
- a `ts_resolution` (instantaneous, daily, etc.)  
- a `ts_name` (raw, corrected, simulated, etc.)  

| id | uuid                                 | code       | thing_id | foi_id | parameter_id | resolution_id | ts_code_id | description                        | valid_from | valid_to |
|----|--------------------------------------|------------|----------|--------|--------------|---------------|------------|------------------------------------|------------|----------|
| 1  | 550e8400-e29b-41d4-a716-446655440090 | Q_D_CORR   | 1        | 1      | 1            | 2             | 2          | Corrected daily discharge at Lagarto | 2009-01-01 |          |
| 2  | 550e8400-e29b-41d4-a716-446655440091 | H_I_RAW    | 1        | 1      | 2            | 1             | 1          | Instantaneous water level (raw)     | 2009-01-01 |          |
| 3  | 550e8400-e29b-41d4-a716-446655440092 | Ctot_D_RECON | 2      | 3      | 4            | 2             | 4          | Reconstructed daily sediment conc.  | 2012-06-15 | 2018-12-31 |
| 4  | 550e8400-e29b-41d4-a716-446655440093 | Q_M_SIM    | 3        | 1      | 1            | 3             | 3          | Modelled monthly discharge (SWAT)   | 2000-01-01 |          |

➡️ Example composition of `code`:  
`<parameter>_<resolution>_<name>` → e.g., **`Q_D_CORR`** = Discharge, Daily, Corrected.  

### Table `data` (Observations)

In WiSSkHy, *data* corresponds to *observations* in the [OGC SensorThings API](https://docs.ogc.org/is/18-088/18-088.html).  
Each observation is a **value linked to a time series** at a given timestamp, optionally with a sensor and a quality flag.  
Timestamps must follow the format `YYYY-MM-DD HH:MM:SS`.

| id | ts_id | date_time           | value  | uncert_abs | uncert_rel | sensor_id | quality_id | mixed_sensor | description                      |
|----|-------|---------------------|--------|------------|------------|-----------|------------|--------------|----------------------------------|
| 1  | 1     | 2025-01-01 00:00:00 | 1420.5 | 5.2        | 0.004      | 1         | 1          | FALSE        | Daily discharge at Lagarto       |
| 2  | 1     | 2025-01-02 00:00:00 | 1387.3 | 4.8        | 0.003      | 1         | 1          | FALSE        |                                  |
| 3  | 2     | 2025-01-01 12:00:00 |   3.45 | 0.05       | 0.015      | 2         | 2          | FALSE        | Instantaneous water level (raw)  |
| 4  | 3     | 2015-07-01 00:00:00 |  220.0 |            |            |           | 3          | derived      | Reconstructed sediment conc.     |

➡️ **Links**:  
- `ts_id` → time series definition (`time_series`)  
- `sensor_id` → measuring device (optional)  
- `quality_id` → quality flag (`quality` table)  
- `mixed_sensor` → TRUE/derived if data come from multiple sources  


### Tables `rating_date`, `rating_hq`, `rating_gms`, `rating_sfd`, `rating_gr`,

These tables define and store **rating curves** (stage–discharge relationships) associated with a given time series.  
A `rating_date` record describes one curve definition, which may be expressed as equations or as point data (`rating_gms`, `rating_hq`).

---

#### Table `rating_date`
Defines the **metadata of a rating curve** linked to a `time_series`.

| id | ts_id | time_start | method       | author     | equation | a   | b   | c   | description                  |
|----|-------|------------|--------------|------------|----------|-----|-----|-----|------------------------------|
| 1  | 1     | 2020-01-01 | Manning-Str. | J. Perez   | YES      | 12  | 1.7 |     | Equation-based rating curve  |
| 2  | 1     | 2022-01-01 | Empirical    | N. Fuertes | NO       |     |     |     | Updated with discrete points |


---

#### Table `rating_hq` (Bijective rating curves)
Stores **(h, q)** support points for *stage–discharge* curves.

| id | rating_date_id | h   | q    |
|----|----------------|-----|-----|
| 1  | 2              | 1.0 | 250 |
| 2  | 2              | 2.0 | 720 |

---

#### Table `rating_gms` (Gauckler Manning Strickler method)
Stores **(h, n)** support points for *GMS-type* rating curves.

| id | rating_date_id | h    | n   |
|----|----------------|------|-----|
| 1  | 1              | 0.5  | 1.2 |
| 2  | 1              | 1.0  | 2.5 |


➡️ **Usage**:  
- `rating_date` → defines the curve and method (equation or empirical).  
- `rating_gms` / `rating_hq` → store support points when the curve is defined empirically.  



---

#### Table `rating_sfd` (Stage Fall Discharge method)
Stores **(h, n)** support points for *GMS-type* rating curves.


---

#### Table `rating_gr` (Gradient method)

---


---

#### Table `sediment_rating` (Sediment Rating Curve)

---




### Table `no_agg_dates`

Defines **periods where aggregation must not be applied** to a given time series (e.g., gaps, anomalies, or special measurement campaigns).  
Linked to a `time_series` via `ts_id`.

| id | ts_id | start_date  | end_date    | description                 |
|----|-------|-------------|-------------|-----------------------------|
| 1  | 1     | 2015-01-01  | 2015-03-31  | Do not aggregate during flood season |
| 2  | 2     | 2018-11-05  | 2018-11-20  | Mobile ADCP campaign        |
| 3  | 1     | 2020-06-10  |             | Ongoing anomaly (sensor drift) |

➡️ **Usage**: these ranges are excluded from automatic daily/monthly/yearly aggregation.


---




## Sampling Tables

These tables describe **sampling events, types, and collected samples** (sediment, geochemical, etc.).  
They allow linking observations (`data`) with physical samples and campaigns.

---

### Table `sampling_event`

Defines a **sampling campaign/event** linked to a `thing` (station or platform).  
Each event has a type, date, and optional metadata.

#### Example records

| id | uuid                                 | sampling_type_id | thing_id | date                | method     | protocol | labo     | comment            |
|----|--------------------------------------|------------------|----------|---------------------|------------|----------|----------|--------------------|
| 1  | 550e8400-e29b-41d4-a716-446655440300 | 1                | 1        | 2025-06-10 10:30:00 | ISO-748    | Grab     | SENAMHI  | High flow sampling |
| 2  | 550e8400-e29b-41d4-a716-446655440301 | 2                | 1        | 2025-06-11 11:00:00 | ISO-4363   | Vertical | ANA      | Integrated sample  |

---

### Table `sampling_type`

Reference table for **types of sampling methods**, with codes organized as `<domain>.<method>[.<variant>]`.

#### Example records

| id | code                  | domain     | label                | default_info_table       | description                          |
|----|-----------------------|------------|----------------------|--------------------------|--------------------------------------|
| 1  | sediment.grab         | sediment   | Grab sampling        | sample_sed               | Single-point sediment grab            |
| 2  | sediment.integrated   | sediment   | Integrated sampling  | sample_sed_integrated    | Depth-integrated sediment sampling    |
| 3  | geochemical.bulk      | geochemical| Bulk geochemical     | sample_geochemical       | Bulk sample for geochemical analysis  |
| 4  | water.surface         | water      | Surface water sample |                          | Bottle sample from surface            |

---

### Table `sampling`

Links a **data value** (from `data`) or sensor to its **sample record**.  
Each sample has a unique `sample_uuid` and may include replicate, vertical, or point identifiers.

#### Example records

| id | sample_uuid | sampling_event_id | sampling_type_id | data_id | sensor_id | no_vertical | no_point | no_replicate | value | comment       |
|----|-------------|-------------------|------------------|---------|-----------|-------------|----------|--------------|-------|---------------|
| 1  | samp-001    | 1                 | 1                | 10      | 2         | 1           | 1        | 1            |  450  | Grab sample   |
| 2  | samp-002    | 2                 | 2                | 11      | 2         | 1           | 1        | 1            |  380  | Integrated    |

---

### Table `sample_sed`

Details of **sediment samples**.

| id | sample_uuid | sampling_event_id | distance_lb | total_depth | depth | lon     | lat     | comment          |
|----|-------------|-------------------|-------------|-------------|-------|---------|---------|------------------|
| 1  | samp-001    | 1                 | 250         | 15          | 7.5   | -73.76  | -10.73  | Mid-channel      |

---

### Table `sample_sed_integrated`

Details of **integrated sediment samples**.

| id | sample_uuid | sampling_event_id | integration_time_seconds | integration_flow | sampler_method | lon     | lat     | comment      |
|----|-------------|-------------------|--------------------------|------------------|----------------|---------|---------|--------------|
| 1  | samp-002    | 2                 | 120                      | 2500             | ISO-4363       | -73.75  | -10.72  | Vertical int |

---

### Table `sample_geochemical`

Details of **geochemical samples**.

| id | sample_uuid | sampling_event_id | pre_treatment | fraction | storage_temp | lab_id | comment              |
|----|-------------|-------------------|---------------|----------|--------------|--------|----------------------|
| 1  | samp-003    | 3                 | freeze-dried  | fine     | -20°C        | 101    | For isotopic analysis |

---

### (Optional) Table `data_sampling_relation`

If enabled, links **observations (`data`) with sampling events**, ensuring consistency between measurements and physical samples.

| id | data_id | sampling_event_id | info     |
|----|---------|-------------------|----------|
| 1  | 10      | 1                 | Matched  |
| 2  | 11      | 2                 | Derived  |




---

## Gauging tables


---

 




# Dates management
txt in the DB











# How to create/add a module?

### Alignment with OGC SensorThings API

The WiSSkHy database architecture is aligned with the [OGC SensorThings API standard](https://docs.ogc.org/is/18-088/18-088.html), which is based on ISO 19156. This alignment ensures interoperability with widely recognized international standards for handling observation and measurement data, and provides a solid foundation for integrating WiSSkHy with other systems and platforms in the hydrological and environmental sciences.

table of correspondance

SQL Query to obtain the Datastream






### Alignment with FAIR Principles

WiSSkHy supports the implementation of the [FAIR principles](https://www.go-fair.org/fair-principles/) (Findable, Accessible, Interoperable, Reusable) for hydrological data.  
By adopting OGC/ISO standards, using open formats, and enabling reproducible workflows, the system helps ensure that data managed with WiSSkHy can be more easily shared, integrated, and reused across research communities.

In particular, the emphasis is placed on:  
- **Interoperability**, by adopting OGC/ISO data models and open formats;  
- **Reusability**, through the use of open-source tools, transparent database structures, and reproducible workflows.  

While SQLite makes WiSSkHy primarily a local tool, future developments (e.g. a PostgreSQL version with server-based access) will further enhance **Accessibility** and **Findability** in multi-user and collaborative contexts.

# Credits

## Coordination
William Santini (IRD - France)

## Conceptualization
- William Santini (IRD - France)
- Alex Delort-Ylla (IRD - France)

## Apps and Database Development
- Alex Delort-Ylla (IRD - France)
- William Santini (IRD - France)
- Andre Luis Martinelli Real dos Santos (SGB - Brazil)

## Team / Contributors to Tools Development
- Alex Delort-Ylla (IRD - France)
- William Santini (IRD - France)
- Andre Luis Martinelli Real dos Santos (SGB - Brazil)

## Technical Advisory
- William Santini (IRD - France)
- Andre Luis Martinelli Real dos Santos (SGB - Brazil)
- Oscar Nain Puita Rodriguez (SENAMHI - Bolivia)
- Nilton Fuertes (SENAMHI - Peru)
- Wilmer Guachamín Acero (INAMHI - Ecuador)



## How to cite
.....

## Funding
This application was developed with the support of:
IRD, SGB, 

## Source Code
Available on GitHub.

## License
This project is licensed under the XXX License.







