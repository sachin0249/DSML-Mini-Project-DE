# UIDAI Aadhaar Data Engineering Project

## 📌 Project Overview

This project is an end-to-end **Aadhaar Data Engineering pipeline** developed using **Microsoft Fabric**.

The project processes Aadhaar-related datasets through a **Medallion Architecture**:

```text
GitHub Raw Data
      ↓
Bronze Layer
      ↓
Silver Layer
      ↓
Gold Layer
      ↓
Data Warehouse / Semantic Model
      ↓
Analytics & Reporting
```

The main objective of this project is to ingest Aadhaar datasets from GitHub, store the raw data in Microsoft Fabric Lakehouse, clean and transform the data using PySpark notebooks, create curated Gold-layer data, and prepare the data for analytical use.

---

# 🏗️ Architecture

```text
                    ┌─────────────────────┐
                    │    GitHub Repository │
                    │    Raw Aadhaar Data  │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │    Bronze Layer     │
                    │    Fabric Lakehouse  │
                    │                     │
                    │ Raw CSV Files       │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │    Silver Layer     │
                    │    Fabric Lakehouse  │
                    │                     │
                    │ Data Cleaning       │
                    │ Data Transformation │
                    │ Data Validation     │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │     Gold Layer      │
                    │  Curated Data Model │
                    │                     │
                    │ Fact / Dimension    │
                    │ Tables & SQL        │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │  Warehouse /         │
                    │  Semantic Model      │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Analytics / Reports  │
                    └─────────────────────┘
```

---

# 🛠️ Technologies Used

* Microsoft Fabric
* Fabric Lakehouse
* Fabric Data Factory / Pipelines
* Apache Spark
* PySpark
* Python
* Delta Lake
* Fabric Warehouse
* SQL
* GitHub
* Power BI / Semantic Model

---

# 📂 Project Structure

```text
DSML-DE/
│
├── Bronze Layer/
│   ├── brone-LH.png
│   ├── bronze-pipeline.png
│   ├── DSML_Bronze_Pipeline.zip
│   └── Get_Git_Data_Notebook.ipynb
│
├── Silver Layer/
│   ├── Silver-LH.png
│   ├── Silver-Pipeline.png
│   ├── Silver_Pipeline.zip
│   ├── Cleaning_boilerPlate.ipynb
│   ├── STATES.csv
│   │
│   └── Notebooks/
│       ├── SILVER_BioCleaning.ipynb
│       ├── SILVER_DemoCleaning.ipynb
│       └── SILVER_EnrolCleaning.ipynb
│
├── Gold Layer/
│   ├── Gold-Pipeline.png
│   ├── Gold-Semantic-Model.png
│   ├── Gold-WH.png
│   ├── gold_pipeline.zip
│   └── MyQueries.zip
│
└── Main Pipeline/
    ├── Main-Pipeline.png
    └── MainPipeline.zip
```

---

# 🥉 Bronze Layer

The Bronze layer is responsible for ingesting the raw Aadhaar datasets into the Fabric Lakehouse.

A GitHub-based ingestion notebook is used to retrieve CSV files from the raw data repository.

### GitHub Data Ingestion

The notebook:

```text
Get_Git_Data_Notebook.ipynb
```

uses the GitHub REST API to identify CSV files inside the `raw` directory and download them into the Fabric Lakehouse.

The ingestion destination is:

```text
Files/raw/
```

The project contains Aadhaar datasets related to:

* Biometric data
* Demographic data
* Enrolment data

The Bronze layer keeps the source data in its raw form before transformation.

### Bronze Components

```text
Bronze Layer
│
├── GitHub Data Source
│
├── Get_Git_Data_Notebook
│
├── Bronze Lakehouse
│
└── Bronze Pipeline
```

---

# 🥈 Silver Layer

The Silver layer performs data cleaning, validation, standardization, and transformation using PySpark.

Three main datasets are processed:

### 1. Biometric Dataset

Notebook:

```text
SILVER_BioCleaning.ipynb
```

The notebook:

* Loads biometric CSV files
* Combines multiple files
* Converts data types
* Converts dates
* Converts pincode to integer
* Converts biometric age-group columns to integer
* Identifies invalid state values
* Removes invalid records
* Standardizes state and district names
* Performs data validation

---

### 2. Demographic Dataset

Notebook:

```text
SILVER_DemoCleaning.ipynb
```

The notebook:

* Loads demographic CSV files
* Combines multiple source files
* Converts date fields
* Converts pincode to integer
* Converts demographic age-group columns to integer
* Removes invalid records
* Standardizes state and district values
* Performs validation and profiling

---

### 3. Enrolment Dataset

Notebook:

```text
SILVER_EnrolCleaning.ipynb
```

The notebook:

* Loads Aadhaar enrolment CSV files
* Combines the source files
* Converts date values
* Converts pincode to integer
* Converts age-group columns to integer
* Removes invalid state records
* Standardizes state and district names
* Performs data validation

---

# 🧹 Data Cleaning

A reusable notebook is also included:

```text
Cleaning_boilerPlate.ipynb
```

It provides a generalized cleaning workflow for different datasets.

The cleaning process includes:

```text
Raw Data
   ↓
Schema Inspection
   ↓
Data Profiling
   ↓
Invalid Value Detection
   ↓
Data Type Conversion
   ↓
Text Normalization
   ↓
Location Standardization
   ↓
Clean Dataset
```

---

# 🔤 Text Normalization

The Silver notebooks contain a reusable text normalization function.

It performs operations such as:

* Converting text to lowercase
* Replacing `&` with `and`
* Replacing spaces with underscores
* Replacing dots and hyphens
* Removing unnecessary underscores
* Removing unwanted characters
* Trimming values

For example:

```text
Before:

Andhra Pradesh
Tamil Nadu
Jammu & Kashmir

After normalization:

andhra_pradesh
tamil_nadu
jammu_and_kashmir
```

This helps maintain consistency when joining datasets and performing analytical queries.

---

# 📍 Location Reference Data

The project also contains:

```text
STATES.csv
```

This reference dataset contains location information such as:

* State
* District
* Pincode

The location data is normalized and converted into the required data types before being used in the transformation process.

---

# 🥇 Gold Layer

The Gold layer contains the curated data model used for analytical purposes.

The project includes:

```text
Gold Pipeline
Gold Warehouse
Gold Semantic Model
SQL Queries
```

The Gold layer transforms cleaned Silver data into an analytical structure.

---

# 🗄️ Gold Data Model

The project contains SQL scripts for creating analytical tables.

Available SQL files include:

```text
Biometric.sql
Demographic.sql
Dim_Date.sql
Dim_location.sql
Enrollment.sql
Fact_Aadhar_Activity.sql
```

The model contains fact and dimension concepts.

### Fact Table

```text
Fact_Aadhar_Activity
```

The fact table represents Aadhaar-related activity and can be analyzed using different dimensions.

### Dimension Tables

```text
Dim_Date
Dim_location
```

These dimensions provide contextual information for analytical queries.

---

# 🔄 Pipelines

The project contains multiple Fabric pipelines.

### Main Pipeline

```text
MainPipeline.zip
```

The Main Pipeline coordinates the overall data engineering workflow.

```text
GitHub
  ↓
Bronze
  ↓
Silver
  ↓
Gold
```

### Bronze Pipeline

```text
DSML_Bronze_Pipeline.zip
```

Responsible for the Bronze-layer ingestion process.

### Silver Pipeline

```text
Silver_Pipeline.zip
```

Responsible for executing the Silver-layer cleaning and transformation workflow.

### Gold Pipeline

```text
gold_pipeline.zip
```

Responsible for preparing the Gold-layer analytical data.

---

# 🔗 End-to-End Workflow

The complete workflow can be represented as:

```text
              GitHub
                │
                ▼
       Raw Aadhaar CSV Files
                │
                ▼
        ┌───────────────┐
        │ Bronze Layer  │
        │ Raw Lakehouse │
        └───────┬───────┘
                │
                ▼
        ┌───────────────┐
        │ Silver Layer  │
        │ PySpark       │
        │ Cleaning      │
        └───────┬───────┘
                │
                ▼
        ┌───────────────┐
        │  Gold Layer   │
        │ Curated Data  │
        └───────┬───────┘
                │
                ▼
        ┌───────────────┐
        │   Warehouse   │
        └───────┬───────┘
                │
                ▼
        ┌───────────────┐
        │ Semantic Model│
        └───────┬───────┘
                │
                ▼
          Analytics
```

---

# 📸 Proof of Work

Screenshots of the Microsoft Fabric implementation are included in the repository.

### Bronze

* Bronze Lakehouse
* Bronze Pipeline

### Silver

* Silver Lakehouse
* Silver Pipeline

### Gold

* Gold Pipeline
* Gold Warehouse
* Gold Semantic Model

### Main Pipeline

* End-to-end Main Pipeline

These screenshots provide visual evidence of the implementation in Microsoft Fabric.

---

# 📊 Key Data Engineering Concepts Demonstrated

This project demonstrates practical implementation of:

* ETL / ELT pipelines
* Data ingestion
* GitHub REST API
* Microsoft Fabric Lakehouse
* Medallion Architecture
* Apache Spark
* PySpark
* Data cleaning
* Data validation
* Data type conversion
* Text normalization
* Data transformation
* Data pipeline orchestration
* Delta Lake
* SQL
* Dimensional modeling
* Fact and dimension tables
* Data Warehouse
* Semantic modeling

---

# 🚀 How to Reproduce the Project

## Step 1 — Prepare the Raw Data

Place the Aadhaar raw CSV datasets in a GitHub repository.

The ingestion notebook retrieves CSV files from the GitHub repository.

---

## Step 2 — Create the Bronze Lakehouse

Create a Lakehouse in Microsoft Fabric and use the Bronze pipeline/notebook to load the raw data.

The raw files are stored under:

```text
Files/raw/
```

---

## Step 3 — Run Silver Transformations

Execute the Silver notebooks:

```text
SILVER_BioCleaning.ipynb
SILVER_DemoCleaning.ipynb
SILVER_EnrolCleaning.ipynb
```

These notebooks clean and standardize the datasets.

---

## Step 4 — Create Gold Data

Execute the Gold pipeline and SQL scripts to create the curated analytical layer.

---

## Step 5 — Create Analytical Model

Use the Gold Warehouse and Semantic Model for analytical reporting.

---

# ⚠️ Important Note

The repository contains project code, pipeline definitions, SQL scripts, notebooks, and screenshots.

Large raw Aadhaar CSV files are **not included directly in this GitHub repository** to avoid unnecessary repository size and duplication of source data.

---

# 👨‍💻 Project

**UIDAI Aadhaar Data Engineering**

Built using:

**Microsoft Fabric | PySpark | Python | SQL | Lakehouse | Data Factory | GitHub**

---

## ⭐ Acknowledgement

This project was developed as a practical implementation of modern data engineering concepts using Microsoft Fabric and Aadhaar-related datasets.

The implementation includes ingestion, transformation, orchestration, warehousing, and analytical modeling.
