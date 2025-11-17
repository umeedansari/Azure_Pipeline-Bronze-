# 📘 Data Engineering Pipeline (Azure Synapse)

This project implements a fully automated **Bronze-layer ingestion pipeline** in Azure Synapse Analytics. The goal of the pipeline is to ingest data incrementally from Azure SQL Database into Azure Data Lake Storage (ADLS) using a metadata-driven approach with CDC (change data capture) logic.

---

## 🔄 Pipelines Overview

### **1. Bronze Alert (Main Pipeline)**
This pipeline acts as the orchestrator.  
Its responsibilities include:
- Triggering the **bronze** ingestion pipeline.
- Monitoring its execution status.
- Triggering a Logic App webhook to send an alert if the ingestion fails.
- Passing metadata such as pipeline name, run ID, and trigger time for tracking and debugging.

This ensures visibility and automated monitoring for the ingestion process.

---

### **2. bronze (Ingestion Pipeline)**
This is the core pipeline responsible for performing **incremental ingestion** for multiple tables from Azure SQL.

#### **How the ingestion works:**

1. **Read table list from GitHub**  
   The pipeline starts by loading a JSON configuration file that defines which SQL tables to ingest.

2. **Loop through each table dynamically**
   For each table in the list:
   - The pipeline reads the last processed **CDC timestamp** stored in ADLS.
   - It queries the SQL database to check whether new or updated rows exist since that timestamp.

3. **If new data is found:**
   - Data is copied from Azure SQL into ADLS (Bronze layer) in **Parquet format**.
   - The pipeline calculates the latest `updated_at` value from SQL.
   - It updates the CDC file in ADLS with the new timestamp so that the next run only loads fresh changes.

This approach allows the pipeline to operate efficiently and continuously without reprocessing old data.  
New tables can be added easily by updating the configuration file—no pipeline changes required.

---

## 🧩 Summary

This project showcases a production-ready **Bronze layer ingestion framework** built in Synapse using:
- Metadata-driven table configuration  
- Incremental CDC-based data loading  
- Dynamic looping and automation  
- ADLS-based watermark (CDC timestamp) tracking  
- Automated failure alerts using Logic Apps  

It provides a scalable, maintainable, and efficient foundation for a modern data lakehouse architecture.

---
