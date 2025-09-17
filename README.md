# WiSSkHy ![Project Progress](https://img.shields.io/badge/Progress-60%25-brightgreen)
Wiki Soft Suite for Hydrology   
Distill your data. Hydrate your science...

## The WiSSkHy framework
**`WiSSkHy` is a local and flexible framework** built with R-Shiny and SQLite for hydrological data management based on an SQLite database and fully open-source tools. Its database architecture is conceptually aligned with the [OGC SensorThings API standard](https://docs.ogc.org/is/18-088/18-088.html), and designed following the [FAIR principles](https://www.go-fair.org/fair-principles/) (Findable, Accessible, Interoperable, Reusable), with a focus on interoperability (OGC/ISO standards) and reusability (open formats, reproducible workflows).

It provides a complete workflow to support hydrological services and researchers, including:

- Data import & storage in a local SQLite database
- Database management (visualize, add, or delete tables and fields)
- Data cleaning
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


## Main tables

### Table `things` 

Station name = infrastructure avec les capteurs et les équipements
A station can be mobile --> see location


### Table `feature_of_interest`
User can define the feature of interest
feature of interest = objet étudié (river, transect, lake, basin, etc.)

CREATE TABLE feature_of_interest (
  foi_id       TEXT PRIMARY KEY,        -- UUID
  name         TEXT NOT NULL,           -- ex. "Station Obidos"
  description  TEXT,                    -- libre
  encodingType TEXT DEFAULT 'application/vnd.geo+json',
  feature      TEXT,                    -- GeoJSON: {"type":"Point","coordinates":[-55.5,-1.9]}
  type         TEXT                     -- optionnel: "station", "cross-section", "basin", "sampling site"
);


### Table current `location`

### Table `historical_location`

### Table `time series`

### Table `data` (Observations)
In WiSSkHy, what is referred to as *data* corresponds to the concept of *observations* in the [OGC SensorThings API](https://docs.ogc.org/is/18-088/18-088.html).  
Each observation consists of a value associated with a parameter, a time, and optionally a sensor and location.


## Configuration tables

It is possible to easily configure the database using the **configuration tables**. These tables are modifiable if needed and ensure the **homogeneity** of the database.

### Table `ts_parameter` (ObservedProperty)

Default **parameters** in the WiSSkHy db:
  - Q = Water discharge  
  - Cst = Total Suspended Sediment Concentration  
  - ...

<img width="1588" height="593" alt="image" src="https://github.com/user-attachments/assets/01a364ff-0140-49f0-ba66-e368a6aef73f" />

### Table `ts_resolution`

Default **time series resolutions**:

| Code   | Label              | Definition                                                                 |
|--------|--------------------|----------------------------------------------------------------------------|
| **I**  | **Instantaneous**  | Raw value without any temporal aggregation (irregular intervals, as recorded). |
| **ss** | **Second**         | Values recorded or aggregated at a second resolution.                      |
| **mm** | **Minute**         | Values recorded or aggregated at a minute resolution.                      |
| **hh** | **Hour**           | Values recorded or aggregated at an hourly resolution.                     |
| **D**  | **Day**            | Values aggregated over 24 hours (e.g., daily mean, max, min, or sum depending on the variable). |
| **M**  | **Month**          | Values aggregated over a calendar month.                                   |
| **Y**  | **Year**           | Values aggregated over a calendar or hydrological year.                    |

Users can define additional custom resolutions if needed (e.g., **mm30** for values aggregated every 30 minutes).

### Table `ts_name`


### Table `sensor`
List of instruments used to collect the data.
ex: Staff gage N°1032_SENAMHI, ADCP RiverPro 600 kHz, ...


### Table `origin`

The `origin` field is used to group families of sensors and to quickly identify the origin of an observation or data record.

Default `origin` codes in the WiSSkHy database:

| Code  | label                                     |
|-------|-------------------------------------------|
| SM    | Staff Measurement                         |
| PS    | Physical Sensor                           |
| WG    | Water Gauging                             |
| DS    | Discrete Sampling                         |
| RS    | Remote Sensing                            |
| M     | Modelling                                 |
| RSM   | Combination of Remote Sensing and Modelling data |
| R     | Reconstructed                             |
| INT   | Interpolated                              |

Users can define additional custom `origin` if needed

### Table `quality`

The table `quality` can be customized according to the quality criteria defined by each institute.  
It allows users to specify standardized codes and their corresponding labels, which can then be used consistently across datasets.

| Code | Label        | Description (optional)                           |
|------|-------------|--------------------------------------------------|
| ok   | OK          | Data is validated and considered reliable        |
| DBT  | Doubtful    | Data may be erroneous or uncertain               |
| P    | Partial     | Data series is incomplete or partially valid     |
| CUM  | Cumulative  | Data represents cumulative values (e.g. totals)  |


### SQL Views

The `WiSSkHy` database uses incremental IDs as primary keys instead of composite keys in order to optimize SQL joins and group operations. To provide more comprehensive and user-friendly access to the data, `WiSSkHy` relies on **SQL views**. 

These views:  
- Expose meaningful relationships between tables without requiring complex joins from the user.  
- Combine metadata (e.g. station, platform, parameter) with observation values.  
- Facilitate queries for analysis, reporting, and data exchange.  
- Are especially convenient for **R-Shiny applications**, since the app can directly query a ready-to-use view instead of building and maintaining complex SQL joins in the server code.  





## `station_view`


## `platform_view`



## `data_view`


# Dates management
txt in the DB

# Getting started with the `WiSSkHy-App`

## Installation
You can either run the Shiny apps with RStudio or use the WiSSkHy.exe. 
If you need to access the WiSSkHy SQLite database externally, you can install DBeaver or SQLite Browser.

## Database

### Start by creating or loading a `WiSSkHy-db`

### Thing (station or platform)

### Time series

### Configuration tables

### Main tables

### Other tables

### Edit field

## Data

### Import
#### Time Series
#### ADCP gauging
#### PSD
#### LISST-SL2
#### Local database
#### Remote Server

### Clean and Export

### Gap filling

### Wrangling

### Summary & Visualization

## Tools

### ADCP

### Rating tools

### Discharge & Flux

### Sediment

#### PSD

#### LISST

#### Turbidity

#### Vertival profiles

#### Modelling

### Remote-sensing

#### Altimetry

#### Water color

#### TRIOS field measurements

### Geochemistry

## Automatic reports Tab

## Query generator Tab





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







