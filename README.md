# 🎵 Spotify End-to-End Data Engineering Project – Data Ingestion Phase

This repository contains the Data Ingestion pipeline built inside **Azure Data Factory (ADF)**. It represents the first phase of an end-to-end cloud data platform for processing Spotify data.

The objective of this pipeline is to orchestrate the automated, incremental data transfer from an operational **Azure SQL Database** into an **Azure Data Lake Storage Gen2 (ADLS Gen2)** environment.

---

## 🏗️ Architecture & Ingestion Design

The architecture is built on enterprise standards, focusing on scalability, resource efficiency, and automated error handling.

### Key Technical Implementations:

1. **Incremental Data Processing (Delta Load):** To avoid the performance overhead of full-table copies, the pipeline uses a watermarking approach (`Last_cdc`). It dynamically detects and extracts only the records that have changed or been added since the last successful execution.

2. **Metadata-Driven Orchestration:**
   The design avoids hardcoded pipelines for individual tables. Instead, it utilizes a parameterized **ForEach Loop** that iterates through an array input (`loop_input`). This ensures high scalability; new tables can be integrated into the ingestion flow simply by updating the JSON metadata parameters, without modifying the pipeline itself.

3. **Automated Error Handling & Alerting (Azure Logic Apps):**
   To ensure high availability and production-level monitoring, the pipeline is integrated with an **Azure Logic App**. In the event of a pipeline failure or unexpected interruption at any activity stage, a conditional routing trigger automatically invokes the Logic App to send real-time failure alerts (e.g., via email), enabling rapid incident response.

4. **CI/CD & Infrastructure as Code (IaC):**
   The development environment is natively integrated with **GitHub** for source control. Deployments and environment syncing are managed via automatically generated **ARM Templates (Azure Resource Manager)**, adhering to professional DevOps practices.

---

## 📸 Pipeline Design (Control Flow)

<img width="1056" height="756" alt="image" src="https://github.com/user-attachments/assets/0ce40259-2012-4ca2-9acd-c5d4953880c5" />

<img width="1088" height="447" alt="image" src="https://github.com/user-attachments/assets/63b3f19a-5f45-4cd2-ba86-be9cc020b867" />



### Component Breakdown:
* **`loop_input` Parameter:** An array defining the table schemas, metadata, and watermark boundaries.
* **`ForEach` Loop:** Coordinates the ingestion process.
* **`Last_cdc` Activity:** Evaluates delta logic for the current iteration.
* **`AzureSQLtoLake` (Copy Activity):** Handles the high-throughput data movement to the landing zone in ADLS Gen2.
* **Failure Path (Not visible in the screenshot):** Configured to execute the **Azure Logic App** webhook if any core activity fails.

---

## 🛠️ Tech Stack & Core Competencies

* **Orchestration & Data Movement:** Azure Data Factory (ADF)
* **Cloud Storage:** Azure Data Lake Storage Gen2 (ADLS Gen2)
* **Source Database:** Azure SQL Database
* **Monitoring & Alerting:** Azure Logic Apps
* **DevOps:** Git, GitHub Integration, ARM Templates (JSON)

---
