# InsuranceMarket_capstone_fabric_data-engineering_project
End-to-end insurance sales analytics pipeline using Microsoft Fabric, PySpark, Delta Lake, Medallion Architecture, and Power BI to analyze conversion funnels, premium revenue, campaigns, channels, and customer retention.
# Insurance Sales & Conversion Funnel Analysis

## 📌 Project Overview

This project implements an end-to-end data engineering and analytics solution for **InsuranceMarket.ae**, a UAE-based insurance brokerage and digital insurance platform.

The solution analyzes the insurance sales funnel, policy purchases, premium revenue, customer retention, channels, products, and campaign performance.

The project follows a **Medallion Architecture (Bronze → Silver → Gold)** to build a scalable and maintainable data pipeline.

##  Business Model
InsuranceMarket.ae earns commission on every policy sold through its platform. Revenue growth depends on two core levers: (1) generating qualified leads through targeted marketing campaigns, and (2) converting those leads efficiently through a structured four-stage sales funnel — Form Submitted → Options Reviewed → Documents Uploaded → Policy Purchased. The platform also runs a customer loyalty programme called myAlfred, which drives repeat engagement and higher-value customer retention.


## 🎯 Business Objectives

* Monitor the customer conversion funnel
* Analyze insurance sales and premium revenue
* Evaluate campaign performance
* Analyze channel and product performance
* Identify repeat customers and retention
* Implement data-quality validation
* Support business reporting through Power BI

## 🏗️ Architecture

```text
Source Files
     │
     ▼
Azure Data Factory / Fabric Pipeline
     │
     ▼
┌──────────────┐
│ Bronze Layer │
│ Raw Data     │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ Silver Layer │
│ Cleaned Data │
│ Delta Tables │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│  Gold Layer  │
│ Dimensional  │
│    Model     │
└──────┬───────┘
       │
       ▼
   Power BI
   Dashboard
```

## 🥉 Bronze Layer

The Bronze layer stores the incoming source data with minimal transformation.

Key implementation points:

* Dynamic source-path parameterization
* File metadata retrieval
* File validation
* Raw data ingestion
* Duplicate prevention
* Audit information
* Pipeline-based processing

The pipeline uses metadata and a dynamic `source_path` rather than relying on a hardcoded source location.

## 🥈 Silver Layer

The Silver layer transforms Bronze data into structured **Delta tables**.

Key activities:

* Data cleansing
* Data type standardization
* Business-rule validation
* Incremental processing
* Error logging
* Delta table creation
* Watermark-based processing

Delta tables are used for the Silver layer, and the project maintains a `silver_error_log` for invalid records rather than simply deleting bad data.

### Incremental Processing

A watermark-based approach is used to avoid reprocessing already-loaded records.

```text
silver_watermark
       │
       ▼
Get last_ingested_at
       │
       ▼
Filter new records
       │
       ▼
Process & Load
       │
       ▼
Find MAX(_ingested_at)
       │
       ▼
Update watermark
```

This approach was one of the key lessons from the project.

## 🥇 Gold Layer

The Gold layer provides business-ready dimensional models for analytics.

It contains dimension and fact tables supporting:

* Customer analysis
* Advisor performance
* Product analysis
* Channel analysis
* Date-based analysis
* Policy analysis
* Conversion funnel analysis

The Gold layer uses surrogate keys and relationships between fact and dimension tables to support analytical reporting.

## 🔄 Channel Mapping & Bridge Table

A channel mapping approach was implemented to standardize campaign channel names and connect campaign data with the Gold dimensional model.

This avoids maintaining large hardcoded `CASE` statements and makes the transformation easier to maintain and audit.

## 📊 Power BI Dashboard

The final dashboard provides insights into:

* Total Leads
* Funnel Stage Conversion
* Policies Purchased
* Total Premium
* Average Premium
* Product Performance
* Channel Performance
* Campaign Conversions
* Repeat Customers
* Customer Retention

### Key Results

| KPI                     |     Result |
| ----------------------- | ---------: |
| Total Leads             |      8,000 |
| Stage 2 Leads           |      6,803 |
| Stage 3 Leads           |      5,123 |
| Policies Purchased      |      3,326 |
| Overall Conversion Rate |     41.58% |
| Total Premium           | AED 15.25M |
| Average Premium         |  AED 3.05K |
| Repeat Customers        |      1,485 |
| Retention Rate          |     61.09% |

The analysis identified 8,000 leads, 3,326 completed policy purchases, approximately AED 15.25M in premium, and a 61.09% retention rate.

## 🧪 Data Quality & Testing

The project includes validation for:

* Funnel stage sequencing
* Premium calculations
* Conversion rates
* Campaign metrics
* Business rules
* Invalid records

For example, invalid funnel date sequences were identified for records including `LEAD007110` and `LEAD007511`.

## 🛠️ Technology Stack

* **Microsoft Fabric**
* **Azure Data Factory / Fabric Data Pipelines**
* **PySpark**
* **Apache Spark**
* **Delta Lake**
* **Lakehouse**
* **SQL**
* **Power BI**
* **Medallion Architecture**
* **Dimensional Data Modeling**

## Repository Structure

```text
insurance-sales-conversion-funnel-analysis/
│
├── README.md
│
├── notebooks/
│   ├── bronze/
│   ├── silver/
│   └── gold/
│
├── pipelines/
│   ├── bronze_pipeline/
│   └── master_pipeline/
│
├── powerbi/
│   └── dashboard/
│
└── presentation/
    └── project_presentation.pptx
```

## 🚀 Key Learning Outcomes

* Built an end-to-end **Bronze → Silver → Gold** data pipeline.
* Implemented incremental data processing using watermarks.
* Worked with Delta tables and Lakehouse architecture.
* Implemented data-quality validation and error logging.
* Applied dimensional modeling for analytical workloads.
* Built business-focused Power BI dashboards.
* Analyzed insurance conversion, premium, campaign, channel, product, and customer-retention metrics.

## 👨‍💻 Author

**Mothies Raj D G**

### Project

**Insurance Sales & Conversion Funnel Analysis**

**Domain:** Insurance / Financial Services
**Focus:** Data Engineering & Business Intelligence

