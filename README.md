# WiSSkHy
Wiki Soft Suite for Hydrology   
Distill your data. Hydrate your science...

## The WiSSkHy R-Shiny Apps
WiSSkHy is a **local** environment built with R-Shiny and SQLite for hydrological data management based on an SQLite database and fully open-source tools.

It provides a complete workflow to support hydrological services and researchers, including:

- Data import & storage in a local SQLite database
- Database management (visualize, add, or delete tables and fields)
- Data cleaning
- Data wrangling
- Data visualization & exploration (interactive plots and tables)
- Data processing and analysis with a suite of tools for:
  - Rating curves
  - ADCP measurements
  - Solid gaugings
  - Flux calculation
  - Remote sensing data processing
  - Geochemistry
- Data export to .csv, .xlsx, etc.
- Automated report generation in LaTeX and HTML

The WiSSkHy Apps were created to provide access to the main tools of the WiSSkHy environment.  
Beyond these core features, users can:
- Add new tools directly in R-Shiny, either within the existing apps or independently.
- Write custom scripts in R, Python, MATLAB, or other languages to work with the WiSSkHy SQLite database.
- Use the apps to automatically generate SQL queries that can be reused in custom code.

> [!NOTE]  
> WiSSkHy not only provides access to core tools but also acts as a collaborative "Wiki":  
> users can contribute, share, and reuse tools or scripts within the community.  
> This flexibility encourages collective development, knowledge exchange, and the integration of custom processing, analysis, or visualization workflows, all while leveraging the common WiSSkHy database structure.

![Schéma](https://github.com/user-attachments/assets/03230a04-b6dd-41fa-8070-0fb65640880b)

# Collaborative Use of the Database

SQLite supports multi-user access, but with important limitations:  
- **Read operations** can be performed simultaneously by multiple users.  
- **Write operations** are exclusive: only one user can modify the database at a time, while others must wait until the operation is completed.  

To facilitate collaboration, WiSSkHy includes a **"Refresh Database"** button so that users can always reload the most recent changes made by others.  

WiSSkHy is primarily designed as a **local tool**, complementing centralized server solutions. It allows researchers to fully leverage the power of **R, Python, MATLAB**, and other tools for data curation, analysis, and scientific research.  

> [!TIP] 
> **Future development**  
> A PostgreSQL version of WiSSkHy is planned, enabling a true multi-user environment with efficient concurrency management.

## Database structure
The WiSSkHy database is based on the **time series concept**. Discrete (punctual, or spot) measurements are also treated as time series, since each measurement is associated with a date.

> [!Important]
>A time series (ts) is defined as the combination of a **parameter** and a **temporal record**:

- Example of **parameters**:  
  - Q = Water discharge  
  - Cst = Total Suspended Sediment Concentration  
  - ...

- Example of **temporal records**:  
  - I = Instantaneous  
  - D = Daily  
  - M = Monthly  
  - Y = Yearly  

For instance, the parameter **Q** (water discharge) can have different temporal resolutions and contexts:  
- **I, D, M, Y** → QA/QC data at Instantaneous, Daily, Monthly, or Yearly steps  
- **Iraw** → Raw data  
- **Ibackup** → Backup data  
- **Ixxx** → Data from the same gauging station but managed by another institution (e.g., NGO, hydrological service, electricity company)  

 When the user creates a station, he must choose and configure a parameter set, with temporal resolutions
 
<img width="1910" height="995" alt="image" src="https://github.com/user-attachments/assets/66e98cc3-4e0f-4d3e-9900-0a31902fd388" />




 It is also possible to manage this of parameters and temporal resolution when needed:


 

### Main tables

#### Stations

#### Time Series

#### Data


### Configuration Tables

It is possible to easily configure the database using the **configuration tables**. These tables are modifiable if needed and ensure the homogeneity of the database.

#### Type of Stations

#### Parameters

<img width="1588" height="593" alt="image" src="https://github.com/user-attachments/assets/01a364ff-0140-49f0-ba66-e368a6aef73f" />

#### Temporal records

#### Origin of the Data

<img width="1607" height="510" alt="image" src="https://github.com/user-attachments/assets/95a7ee5d-13e4-4a43-8165-c8f2d3fe8581" />

#### Quality of the Data

<img width="1600" height="327" alt="image" src="https://github.com/user-attachments/assets/95ae4ef9-2f3b-47e0-bb40-7995f8cc9890" />


## Installation
You can either run the Shiny apps with RStudio or use the WiSSkHy.exe. 
If you need to access the WiSSkHy SQLite database externally, you can install DBeaver or SQLite Browser.

## Database

### Start by creating or loading a WiSSkHy database

<img width="1907" height="887" alt="image" src="https://github.com/user-attachments/assets/9a990657-91f9-41e6-b531-38f15d1023e9" />


### Create a new station


### Create a new parameter set

## Data

### Import

### Export or delete

### Data cleaning

### Data wrangling

### Data analysis



## Advanced tools

### ADCP

### Rating tools

### Discharge & Flux

### Remote-sensing

### Geochemistry

### External tools

### Automatic reports

## Query generator






