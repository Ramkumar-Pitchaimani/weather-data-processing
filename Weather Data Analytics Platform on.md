# 🌦️ Weather Data Analytics Platform on Google Cloud

## Overview

The **Weather Data Analytics Platform on Google Cloud** is an end-to-end data engineering solution that automates the ingestion, transformation, and analytics of weather forecast data using Google Cloud Platform (GCP).

The pipeline periodically retrieves weather forecast data from an external REST API, validates the data, stores raw files in Google Cloud Storage (GCS), processes the data using Apache Spark on Dataproc Serverless, and loads curated data into BigQuery for analytics and reporting.

This project demonstrates modern cloud-native data engineering practices including workflow orchestration, scalable ETL processing, cloud storage, and analytical reporting.

---

# Business Problem

Many industries depend on weather forecasts for operational planning and business decisions.

Typical use cases include:

- Logistics & Supply Chain
- Airlines
- Agriculture
- Retail
- Smart Cities
- Disaster Management
- Energy & Utilities

Instead of manually downloading weather reports every day, organizations require an automated platform capable of continuously ingesting, processing, and storing weather data for historical analysis and reporting.

This project demonstrates such an enterprise-grade solution.

---

# Solution Architecture

```
                    +-------------------------+
                    |   Weather REST API      |
                    +-----------+-------------+
                                |
                        Fetch Forecast Data
                                |
                                ▼
                  +---------------------------+
                  |   Apache Airflow DAG 1    |
                  +---------------------------+
                  | • Fetch API Data          |
                  | • Validate Response       |
                  | • Store CSV in GCS        |
                  +------------+--------------+
                               |
                               ▼
               +-------------------------------+
               | Google Cloud Storage (Landing)|
               | forecast/yyyy-mm-dd/*.csv     |
               +---------------+---------------+
                               |
                               ▼
                  +---------------------------+
                  |   Apache Airflow DAG 2    |
                  +---------------------------+
                  | Trigger Spark Processing  |
                  +------------+--------------+
                               |
                               ▼
               +-------------------------------+
               | Dataproc Serverless (Spark)   |
               +-------------------------------+
               | • Read CSV from GCS           |
               | • Clean Data                  |
               | • Validate Schema             |
               | • Apply Business Rules        |
               | • Transform Data              |
               +---------------+---------------+
                               |
                               ▼
                     +----------------------+
                     |      BigQuery        |
                     +----------------------+
                               |
                               ▼
                  Analytics / Dashboards / SQL
```

---

# Technology Stack

| Layer | Technology |
|--------|------------|
| Cloud Platform | Google Cloud Platform |
| Programming Language | Python |
| Workflow Orchestration | Apache Airflow |
| Object Storage | Google Cloud Storage |
| Distributed Processing | Apache Spark |
| Compute | Dataproc Serverless |
| Data Warehouse | BigQuery |
| Source System | Weather REST API |
| Analytics | SQL / Looker Studio |

---

# Project Workflow

## Step 1 – Fetch Weather Data

Apache Airflow DAG 1 invokes the Weather REST API at scheduled intervals.

Example API Request

```
GET /forecast
```

The API returns forecast data in JSON format.

---

## Step 2 – Data Validation

Incoming data is validated before storage.

Validation includes:

- Mandatory field validation
- Null value checks
- Duplicate detection
- Timestamp validation
- Schema validation

Only valid records continue through the pipeline.

---

## Step 3 – Store Raw Data

Validated forecast data is converted into CSV format and stored in Google Cloud Storage.

Example folder structure:

```
forecast/

    2026-06-28/
        forecast.csv

    2026-06-29/
        forecast.csv
```

This layer acts as the **Landing Zone** for raw data.

---

## Step 4 – Spark Processing

Apache Airflow DAG 2 triggers a Dataproc Serverless Spark job.

The Spark application performs:

- Data Cleansing
- Data Type Conversion
- Date Standardization
- Duplicate Removal
- Schema Validation
- Business Rule Implementation

This creates curated datasets suitable for analytics.

---

## Step 5 – Load Data into BigQuery

The transformed data is loaded into BigQuery for analytical reporting.

Example Dataset

```
weather_dw
```

Example Table

```
forecast
```

---

# BigQuery Schema

| Column | Data Type |
|----------|-----------|
| forecast_date | DATE |
| city | STRING |
| country | STRING |
| temperature | FLOAT |
| humidity | FLOAT |
| pressure | FLOAT |
| wind_speed | FLOAT |
| weather_condition | STRING |
| ingestion_timestamp | TIMESTAMP |

---

# Sample SQL Queries

## Average Temperature by City

```sql
SELECT
    city,
    AVG(temperature) AS average_temperature
FROM weather_dw.forecast
GROUP BY city
ORDER BY average_temperature DESC;
```

---

## Daily Forecast Count

```sql
SELECT
    forecast_date,
    COUNT(*) AS total_records
FROM weather_dw.forecast
GROUP BY forecast_date;
```

---

## Highest Recorded Temperature

```sql
SELECT
    city,
    MAX(temperature) AS highest_temperature
FROM weather_dw.forecast
GROUP BY city;
```

---

# Airflow Workflows

## DAG 1 – Weather Data Ingestion

Purpose

- Fetch weather forecast from API
- Validate incoming data
- Store CSV in Google Cloud Storage

Workflow

```
Fetch API
      ↓
Validate Data
      ↓
Upload CSV to GCS
```

---

## DAG 2 – Spark Processing

Purpose

- Execute Spark ETL job
- Transform raw weather data
- Load curated data into BigQuery

Workflow

```
Run Dataproc Serverless Job
          ↓
Read CSV
          ↓
Transform Data
          ↓
Load BigQuery
```

---

# Repository Structure

```
weather-data-platform/

│
├── dags/
│     ├── weather_ingestion_dag.py
│     ├── spark_processing_dag.py
│
├── spark/
│     ├── weather_transform.py
│
├── api/
│     ├── fetch_weather.py
│
├── sql/
│     ├── create_tables.sql
│
├── architecture/
│     ├── architecture.png
│
├── screenshots/
│
├── sample_data/
│
├── README.md
```

---

# Features

- Automated Weather API Ingestion
- Workflow Orchestration using Apache Airflow
- Cloud Storage Landing Zone
- Distributed Processing using Apache Spark
- Serverless Processing with Dataproc
- BigQuery Data Warehouse
- SQL Analytics
- Scalable ETL Architecture
- Production-style Pipeline Design

---

# Skills Demonstrated

- Google Cloud Platform
- Apache Airflow
- Google Cloud Storage
- Apache Spark
- Dataproc Serverless
- BigQuery
- Python
- REST API Integration
- ETL Pipeline Development
- Data Validation
- Workflow Automation
- Cloud Data Engineering

---

# Future Enhancements

This solution can be further extended with:

- Event-driven architecture using Cloud Functions
- Pub/Sub streaming ingestion
- Data Quality Framework
- CI/CD using Cloud Build
- Infrastructure as Code using Terraform
- Cloud Monitoring & Alerting
- Slack/Email Notifications
- Unit Testing
- Docker Containerization
- Looker Studio Dashboard
- Data Lineage
- Audit Logging

---



