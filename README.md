# GCP-HEALTHCARE-SERVERLESS (Serverless Healthcare RCM Data Lakehouse)

This is an end-to-end **GCP Data Engineering portfolio project** built for the **Healthcare Revenue Cycle Management (RCM)** domain.  
It demonstrates how to design a **serverless data lakehouse** using **GCS + BigQuery** with **Medallion Architecture (Bronze → Silver → Gold)** and orchestration using **Cloud Scheduler + Cloud Function (Gen2)**.

---

## Project Objective

Healthcare organizations receive large volumes of **insurance claims**. This project centralizes and transforms that data so hospitals can monitor:

- total claims per hospital
- approved vs rejected claims
- approved revenue
- approval rate
- patient billing summary

---

## Tech Stack Used (Serverless)

- **Google Cloud Storage (GCS)** – landing/raw zone for CSV files
- **BigQuery** – Bronze/Silver/Gold datasets + transformations
- **Cloud Function Gen2** – pipeline controller (serverless orchestration)
- **Cloud Scheduler** – triggers pipeline daily
- **Cloud Build + GitHub** – CI/CD automation
- **Cloud Logging** – logs and monitoring

---

## Data Sources

### 1) Claims Data (Monthly CSV)
- Hospital A claims file
- Hospital B claims file

Example files:
- `claim_hospital_a_202503.csv`
- `claim_hospital_b_202503.csv`

### 2) CPT Codes (Reference/Constant CSV)
Standard medical procedure codes used for healthcare billing.

File:
- `cpt_codes.csv`

---

## Architecture (High Level Flow)

Cloud Scheduler triggers the Cloud Function daily:

Cloud Scheduler
↓
Cloud Function Controller
↓
Load GCS → BigQuery Bronze
↓
Transform Bronze → Silver (CDM)
↓
Transform Silver → Gold (KPIs)


---

## GCS Folder Structure

Bucket: `gs://revcycle-lite-bucket/`

landing/
claims/2025/03/
claim_hospital_a_202503.csv
claim_hospital_b_202503.csv
cptcodes/
cpt_codes.csv


---

## BigQuery Datasets (Medallion Layers)

### Bronze (Raw)
Dataset: `revcycle_bronze`

Tables:
- `bronze_claims`
- `bronze_cpt_codes`

### Silver (Clean + CDM)
Dataset: `revcycle_silver`

Tables:
- `fact_claim`
- `fact_transaction`
- `fact_encounter`
- `dim_patient`

### Gold (Business)
Dataset: `revcycle_gold`

Tables:
- `gold_revenue_kpis`
- `gold_patients_summary`

---

## Gold KPI Outputs

### `gold_revenue_kpis`
Hospital-level KPIs:
- total claims
- total claimed revenue
- total approved revenue
- approval rate
- approved / rejected / pending counts

### `gold_patients_summary`
Patient-level KPIs:
- total encounters
- total billed vs paid
- pending amount
- last encounter date

---

## CI/CD (Production style)

Whenever code is pushed to GitHub:

GitHub Push
↓
Cloud Build Trigger
↓
Run tests (/tests)
↓
Deploy Cloud Function


tests prevents deploying broken SQL/code.

---

## Summary

> “Built a serverless Healthcare RCM data lakehouse on GCP. Claims and CPT reference data land in GCS, then Cloud Scheduler triggers a Cloud Function controller daily, which loads Bronze tables in BigQuery, transforms them into Silver CDM tables, and generates Gold KPI tables for analytics dashboards. Implemented CI/CD with Cloud Build and included test automation for quality.”

---
