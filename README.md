# DBT End-to-End Netflix Data Analysis Project

This project provides an end-to-end solution focusing on the **Data Build Tool (DBT) & Snowflake**, demonstrated through a data transformation workflow using a simulated Netflix scenario.

## 1. Project Goals

The primary objectives of this project are :

*   **Learn DBT in Core:** Master the foundational concepts and capabilities of DBT .
*   **Transformation Focus:** Utilize DBT to write modular SQL queries and apply business logic directly within the data warehouse .
*   **Create Analytics-Ready Data:** Transform raw source data into structured, analytics-ready data models .
*   **Upskill:** Acquire practical skills highly valuable for placement and professional portfolios .

## 2. Architecture and Data Flow (ELT)
The project leverages a simple architecture diagram centered around the ELT concept, where transformation occurs within the data warehouse.

* **Data Source** - The MovieLens dataset, used to simulate Netflix data for domain analysis.
* **Storage (L)** - Amazon S3 is used for uploading the raw data (CSV files).
* **Data Warehouse (L)** - Snowflake serves as the data platform where data is automatically loaded, establishing the "raw landing zone". Snowflake is designed for storing and analyzing large volumes of structured data.
* **Transformation (T)** - DBT (Data Build Tool) connects directly to the data warehouse (Snowflake) to execute transformation logic.
* **Serving Layer** - Transformed data is connected to BI tools, such as Looker Studio, PowerBI, or Tableau, to create dashboards and share analytical reports with stakeholders.

ELT Workflow Summary
The data flow adheres to the ELT concept, which reverses the traditional ETL order by performing the loading step before transformation:
1. Extract: Data (e.g., MovieLens files) is extracted.
2. Load: The data is loaded into the storage (Amazon S3) and then onto the Snowflake database.
3. Transform: DBT connects to Snowflake and allows users to write modular SQL queries to transform the raw data into analytics-ready models.

## 3. Data Warehouse Structure (Snowflake Layers)

DBT models are typically organized into distinct layers within the data warehouse (Snowflake) to ensure governance and maintain raw data integrity:

*   **Raw Landing Zone (Raw Schema):** Holds raw, untouched data loaded directly from the source (e.g., S3).
*   **Staging Layer (Views/Tables):** Used for light cleaning, such as renaming columns or basic type casting. This layer acts as a copy of the raw data.
*   **Warehouse Layer (DIM/FACT Tables):** Contains fully structured and transformed data, often following dimensional modeling principles.
*   **Mart Layer (Views/Tables):** Final aggregated models created for specific downstream use cases or stakeholder reporting.

## 4. Key DBT Concepts

DBT brings software engineering best practices (modularity, testing, documentation) to data transformation.


* **Models** - The core of DBT; simple SQL files (`.sql` scripts) containing a single `SELECT` statement that defines transformation logic. Models can **reference other models** to automatically create a dependency graph, managing execution order. 
* **Materialization** - Strategies defining how DBT persists models in the data warehouse. Options include: **View** (stores query, latest data), **Table** (stores data, fast querying), **Incremental** (updates records since the last run, useful for upsert/partial loading), and **Ephemeral** (used only as a CTE in dependent models, no physical object created). 
* **Snapshot** - DBT's built-in implementation of **Type 2 Slowly Changing Dimensions (SCD2)**. Snapshots track and record the history of mutable rows over time, typically using `dbt_valid_from` and `dbt_valid_to` columns. 
* **Testing** - Used to ensure data quality. Includes **Generic Tests** (e.g., `unique`, `not null`, `relationship`, `accepted values`) and **Singular Tests** (custom SQL for specific business logic checks).
* **Documentation** - Automatically generates a comprehensive documentation website, including model descriptions, column details, and a visual **lineage graph** showing dependencies. 
* **Seeds** - Used to quickly load small, static reference CSV files (e.g., lookup tables) directly from the local system into the data warehouse as a table. 
* **Sources** - Configuration (`sources.yml`) used to define and document the raw data tables already loaded into the data warehouse, enabling documentation and freshness checks on raw data. 
* **Macros** - Functions written in SQL/Jinja that encapsulate reusable SQL logic to increase modularity and avoid repetitive code. 
* **Packages** - Collections of reusable models, macros, and tests, similar to software libraries, used to leverage community functionality (e.g., `dbt_utils` for surrogate key generation). 
* **Analysis** - SQL queries used for ad-hoc analysis, exploratory data analysis, or generating reports; these files do not create persistent objects in the data warehouse. 

## 5. Prerequisites

To execute this project, the following are required:

*   **SQL Knowledge:** Basic understanding of SQL is essential (writing `SELECT` statements, using CTEs).
*   **Cloud Accounts:** Access to a **Snowflake** account (30-day free trial available) and an **AWS** account (to set up an S3 bucket).
*   **DBT Core Setup:** DBT Core and the `dbt-snowflake` connector must be installed locally, ideally within a Python virtual environment.
*   **DBT Profile:** A root DBT profile must be configured to connect the local project to the Snowflake data warehouse.
<img width="2816" height="1536" alt="Data Pipeline" src="https://github.com/user-attachments/assets/9e13843c-2283-4f03-9d8b-9a02ce3a39ae" />
