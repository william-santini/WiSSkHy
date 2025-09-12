# WiSSkHy ![Project Progress](https://img.shields.io/badge/Progress-60%25-brightgreen)
Wiki Soft Suite for Hydrology   
Distill your data. Hydrate your science...

## The WiSSkHy R-Shiny Apps
**WiSSkHy is a local and flexible framework** built with R-Shiny and SQLite for hydrological data management based on an SQLite database and fully open-source tools. Its database architecture is aligned with the [OGC SensorThings API standard](https://docs.ogc.org/is/18-088/18-088.html), and designed following the [FAIR principles](https://www.go-fair.org/fair-principles/) (Findable, Accessible, Interoperable, Reusable), with a focus on interoperability (OGC/ISO standards) and reusability (open formats, reproducible workflows).

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

The **WiSSkHy Apps** were created to provide access to the main tools of the WiSSkHy environment.  
Beyond these core features, users can:
- Add new tools directly in R-Shiny, either within the existing apps or independently.
- Write custom scripts in R, Python, or other languages to work with the WiSSkHy SQLite database.
- Use the apps to automatically generate SQL queries that can be reused in custom code/script.

> [!NOTE]  
> WiSSkHy not only provides access to core tools but also acts as a collaborative "Wiki":  
> users can contribute, share, and reuse tools or scripts within the community.  
> This flexibility encourages collective development, knowledge exchange, and the integration of custom processing, analysis, or visualization workflows, all while leveraging the common WiSSkHy database structure.

![Schéma](https://github.com/user-attachments/assets/03230a04-b6dd-41fa-8070-0fb65640880b)

## Collaborative use of the database

SQLite supports multi-user access, but with important limitations:  
- **Read operations** can be performed simultaneously by multiple users.  
- **Write operations** are exclusive: only one user can modify the database at a time, while others must wait until the operation is completed.  

To facilitate collaboration, WiSSkHy includes a **"Refresh Database"** button so that users can always reload the most recent changes made by others.  

WiSSkHy is primarily designed as a **local tool**, complementing centralized server solutions. It allows researchers to fully leverage the power of **R, Python**, and other tools for data curation, analysis, and scientific research.  

> [!TIP]  
> **Future development**  
> A **PostgreSQL** version of WiSSkHy is planned, enabling a true multi-user environment with efficient concurrency management.  
> This version will follow all the FAIR principles (Findable, Accessible, Interoperable, Reusable).


## Project status

WiSSkHy is actively under development. The current progress is estimated at **60% complete**.

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
> WiSSkHy continues to evolve as a collaborative tool for hydrological data management, aiming to leverage the power of R, Python, and other languages while ensuring reproducible and flexible workflows.


## Database structure

WiSSkHy manages stations with observations (or data) coming from sensors (e.g., limnigraphs, ADCP) as well as derived data (e.g., discharge, sediment fluxes). Since derived data are not directly produced by sensors, WiSSkHy relies on the concept of **time series** rather than direct **datastreams** (as in the SensorThings norm). 
However, each time series can be associated with a sensor when relevant, and the database structure makes it possible to map WiSSkHy tables to the [OGC SensorThings API standard](https://docs.ogc.org/is/18-088/18-088.html). WiSSkHy can therefore be considered as **aligned with the SensorThings model**, while remaining flexible enough to handle both raw and derived hydrological data.

Discrete measurements (punctual or spot samples) are also handled as time series, since each measurement is associated with a date.

> [!Important]
> A **time series (ts)** is defined as the combination of a **parameter** (Observed Property) and a **temporal record**:  
> [time series] = [ts_parameter] + [ts_temporal_record]  
> Where:  
> [temporal record] = [ts_name] + [ts_temporal_resolution]

- Example of **parameter**:  
  - Q = Water discharge  
  - Cst = Total Suspended Sediment Concentration  
  - ...

- Example of **time series resolution**

| Code | Label                | Definition                                                                 |
|------|----------------------|----------------------------------------------------------------------------|
| **I** | **Instantaneous**   | Value without any temporal aggregation (e.g., sub-hourly, hourly, or irregular intervals). |
| **D** | **Daily**           | Values aggregated over 24 hours (e.g., daily mean, max, min, or sum depending on the variable). |
| **M** | **Monthly**         | Values aggregated over a calendar month.                                  |
| **Y** | **Yearly**          | Values aggregated over a calendar or hydrological year.                                   |





For instance, the parameter **Q** (water discharge) can have different temporal resolutions and contexts:  
- **I, D, M, Y** → QA/QC data at Instantaneous, Daily, Monthly, or Yearly steps  
- **Iraw** → Raw data  
- **Iclean** → Cleaned data 
- **Ixxx** → Data from the same gauging station but managed by another institution (e.g., NGO, hydrological service, electricity company)





 When the user creates a station, he must choose and configure a parameter set, with temporal resolutions and sensor (origin).
 
<img width="1910" height="995" alt="image" src="https://github.com/user-attachments/assets/66e98cc3-4e0f-4d3e-9900-0a31902fd388" />




 It is also possible to manage this of parameters and temporal resolution when needed:


 

### Main tables

#### Station (Things)

#### Location

#### Time Series

#### Data (Observations)
In WiSSkHy, what is referred to as *data* corresponds to the concept of *observations* in the [OGC SensorThings API](https://docs.ogc.org/is/18-088/18-088.html).  
Each observation consists of a value associated with a parameter, a time, and optionally a sensor and location.

### Configuration Tables

It is possible to easily configure the database using the **configuration tables**. These tables are modifiable if needed and ensure the **homogeneity** of the database.

#### Table 'featureofinterest' (Station Type)
User can define the feature of interest of the station, which is equivalent to the station type.
River, Lake, Meteo...


#### Table 'parameter' (ObservedProperty)

<img width="1588" height="593" alt="image" src="https://github.com/user-attachments/assets/01a364ff-0140-49f0-ba66-e368a6aef73f" />

#### Table 'temporal_record'

#### Table 'sensor'

#### Table 'origin'

- Example of origin
  - SM: Staff Meausurement
  - S: Sensor
  - RS: Remote Sensing
  - M: Modelling
  - RSM: Combination of Remote Sensing and Modelling data
  - R: Reconstitued
  - INT: Interpolatted



<img width="1607" height="510" alt="image" src="https://github.com/user-attachments/assets/95a7ee5d-13e4-4a43-8165-c8f2d3fe8581" />

#### Table 'quality'

<img width="1600" height="327" alt="image" src="https://github.com/user-attachments/assets/95ae4ef9-2f3b-47e0-bb40-7995f8cc9890" />

# Getting started with the WiSSkHy Apps

## Installation
You can either run the Shiny apps with RStudio or use the WiSSkHy.exe. 
If you need to access the WiSSkHy SQLite database externally, you can install DBeaver or SQLite Browser.

## Database tab

### Start by creating or loading a WiSSkHy database

<img width="1907" height="887" alt="image" src="https://github.com/user-attachments/assets/9a990657-91f9-41e6-b531-38f15d1023e9" />


### Create a new station/Thing


### Create a new parameter set

## Data Tab

### Import

### Export or delete

### Cleaning

### Wrangling

### Summary & Visualization



## Tools Tab

### Hydro

#### ADCP

#### Rating tools

#### Discharge & Flux

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







