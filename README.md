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

The WiSSkHy Apps was created to provide access to the main tools of the WiSSkHy environment. Users can also:
- Add new tools directly in R-Shiny, either within the existing Apps or independently.
- Write custom scripts in R, Python, MATLAB, or other languages to work with the WiSSkHy SQLite database.
- Use the Apps to automatically generate SQL queries that can be reused in custom code.

::: note
This flexibility allows users to integrate their own processing, analysis, or visualization workflows while leveraging the common WiSSkHy database structure.
:::


![Schéma](https://github.com/user-attachments/assets/03230a04-b6dd-41fa-8070-0fb65640880b)

## Database Structure
The WiSSkHy database is based on the **time series concept**. Discrete (punctual, or spot) measurements are also treated as time series, since each measurement is associated with a date.

A time series (ts) is defined as the combination of a **parameter type** and a **temporal resolution**:

- Example of parameter types:  
  - Q = Water discharge  
  - Ct = Total Suspended Sediment Concentration
  - ...

- Temporal resolution of the time series:  
  - I = Instantaneous  
  - D = Daily  
  - M = Monthly  
  - Y = Yearly  

 When the user creates a station, they must choose and configure a parameter set:


 

### Main tables


### Annex Tables

It is possible to easily configure the database using the **Annex tables**. These tables are modifiable if needed and ensure the homogeneity of the database.

#### Type of Stations

#### Type of Time Series

#### Origin of the Data

#### Quality of the Data

#### Type of Parameters










## Installation
You can either run the Shiny apps with RStudio or use the WiSSkHy.exe. 
If you need to access the WiSSkHy SQLite database externally, you can install DBeaver or SQLite Browser.

## Database management

![image](https://github.com/user-attachments/assets/d1e9c9dd-d984-45d8-b551-fcca412655c0)
  
![image](https://github.com/user-attachments/assets/81cc87d5-0a6f-4f83-92d0-a5151a946691)
  
![image](https://github.com/user-attachments/assets/f48482c7-6c99-40d2-8ad3-a6adcd3dbc8e)
  
![image](https://github.com/user-attachments/assets/2059dbba-1017-447b-a18a-47fda594a6cc)
  
![image](https://github.com/user-attachments/assets/37fd623e-2f08-4731-aea2-f100496b4a10)

### Create a new station


### Create a new parameter set

## Data management


## Tools

