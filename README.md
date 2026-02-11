
# NYC Mobility Data Pipeline: A Data Management Approach

## Project Overview

This project implements a comprehensive **Data Management** architecture to govern the full lifecycle of urban mobility information in New York City. By integrating heterogeneous data sources—including bike-sharing transactions, meteorological records, and professional sports schedules—the pipeline reconciles disparate datasets into a high-quality **Analytical Base Table (ABT)** for downstream predictive modeling. 

The central research objective was to quantify how exogenous variables, such as adverse weather and high-profile sporting events (NY Knicks), influence urban transportation demand. 

---

## Data Management & Lifecycle

### 1. Data Acquisition & Heterogeneity

The architecture manages three distinct and structurally heterogeneous sources:

**Transactional Data**: Historical Citi Bike trip logs processed via batch-oriented ingestion of large CSV files. 
**Meteorological Data**: Environmental variables (temperature, precipitation, wind) retrieved from the **Open-Meteo REST API**. 
**Event Data**: NY Knicks home/away schedules acquired through **Dynamic Web Scraping** using **Selenium** to handle asynchronous HTML rendering. 
### 2. Robustness & Ingestion Control

To ensure the continuity and reliability of the data flow, the pipeline incorporates:
**Persistent Caching**: API responses are stored locally in a SQLite cache to avoid redundant network calls. 
**Retry Logic**: Automated request attempts with exponential backoff to manage network instabilities.  
**Browser Automation**: Explicit wait mechanisms to ensure data integrity during dynamic scraping. 
### 3. Conflict Resolution & Preparation
The "Reconciliation" phase addresses critical data conflicts:
**Timezone Alignment**: Normalization of weather timestamps from UTC to local EST (America/New_York) to maintain temporal consistency with mobility logs.  
**Granularity Standardization**: Resolving conflicts between point-in-time events (game starts) and hourly demand buckets through specialized rounding logic. 
**Semantic Enrichment**: Implementation of a **Temporal Window Expansion** algorithm to capture the impact of events across arrival and exit phases ( to  hours). 
### 4. Data Quality Assessment
Continuous quality checks are integrated into the pipeline:
**Completeness**: Addressing "blind spots" by extending the API acquisition window to cover timezone-related gaps. 
**Semantic Accuracy**: Filtering "physiological" errors, such as anomalous trips shorter than 1 minute or longer than 3 hours. 
**Integrity**: Using a **Surrogate Primary Key** () to enforce uniqueness and prevent redundancy during incremental storage. 

---

## Storage & Validation
### Data Model
The reconciled data is historicized using a **Relational Model** on a **SQLite DBMS**. This architecture ensures structural integrity and atomicity, providing an optimized **Analytical Base Table (ABT)** where the target variable is discretized into demand classes (Low, Medium, High). 
### Key Findings
**Event Impact**: Integrated data confirms that Knicks home games generate an average traffic volume **32% higher** than the baseline. 
**Environmental Drivers**: Temperature was identified as the primary exogenous driver of demand with a positive correlation (). 

---
## Technologies Used
**Language**: Python 
**Data Handling**: Pandas, NumPy 
**Acquisition**: Selenium (Web Scraping), Requests (REST API) 
**Storage**: SQLite3 
**Visualization**: Seaborn, Matplotlib 


