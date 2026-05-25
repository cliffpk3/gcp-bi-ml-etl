# SUPERSTORE Automotive Financial Forecasting & Analytics Platform

<p align="center">
  <img src="https://github.com/cliffpk3/gcp-bi-ml-etl/blob/main/img/superstore-logo.png?raw=true" alt="SUPERSTORE" width="300">
</p>

## 📋 Project Description

This project delivers an End-to-End Business Intelligence and Predictive Analytics Platform designed for **SUPERSTORE**, a multinational retail chain within the automotive segment. The primary objective is to centralize high-volume historical sales data, execute advanced data engineering and cleansing pipelines, and deploy Machine Learning models to generate a 90-day revenue forecast, empowering FP&A (Financial Planning & Analysis) teams with actionable insights for strategic decision-making.

The solution encompasses everything from raw cloud data ingestion to the deployment of a highly dynamic, interactive executive dashboard on the Power BI Service, leveraging dimensional modeling (Star Schema), FinOps principles, and rigorous data governance.

---

## 🛠️ Tech Stack & Technical Learnings

A core pillar of this project was diving deep into and practically applying modern cloud data stack components and analytical modeling frameworks:

### ☁️ Google Cloud Platform (GCP) & Cloud Storage
* **Data Lake Layer:** Utilized Google Cloud Storage as the centralized repository for raw data files (Raw/Bronze Layer).
* **Document AI & OCR:** Explored automated document intelligence and metadata extraction pipelines from unstructured source files (Invoices).
* **Cloud Regional Infrastructure Management:** Troubleshooted and resolved cloud-to-BI metadata communication issues (*Region Mismatch*) by migrating and unifying the datasets from Single-Region (`us-east1`) to **Multi-Region (US)**, ensuring native compatibility and global performance.

### ⚡ BigQuery & BigQuery ML (Machine Learning)
* **Predictive Modeling (ARIMA_PLUS):** Implemented advanced time-series forecasting models natively inside the Data Warehouse environment using BigQuery ML (`ML.FORECAST`).
* **Training Bias Mitigation (Data Cleansing):** Engineered an upstream SQL intervention to handle truncated data from an incomplete final month (August 2017), preventing autoregressive features from forcing a false steep downward trend into future projections.
* **Multidimensional Scenario Simulation:** Developed robust SQL transformation logic (`CROSS JOIN UNNEST`) to dynamically distribute and simulate forecast values across multidimensional grain granularities (cross-joining store branches and product families).

### 📊 Power BI & Advanced DAX (Dimensional Analytics)
* **FinOps Architecture (Import Mode):** Opted for *Import* storage mode over *DirectQuery* as a cost-optimization design pattern. Data is cached within the local VertiPaq engine during a single scheduled daily refresh, eliminating high, on-demand BigQuery query scan costs.
* **Virtual Tables & Custom Keys via DAX:** Built a customized standard time dimension (`dCalendar`) from scratch, embedding a rigid numerical chronological sort key (`MonthYearKey` in `YYYYMM` format) to override default alphabetical axis sorting behavior.
* **What-If Analysis (Financial Simulator):** Implemented dynamic parameters enabling executive users to manipulate percentage modifiers and test optimized revenue simulation scenarios on the fly.
* **Advanced Context Evaluation & Iterators:**
  * `SWITCH` + `SELECTEDVALUE`: Tied together with disconnected tables to dynamically switch between metrics and scenarios within a single visual frame.
  * `MAXX` + `SUMMARIZE` + `ALLSELECTED`: Overrode the evaluation filter context imposed by the line chart's X-axis. This advanced table-in-memory approach enables dynamic markers that highlight **only** the global peak revenue (`MAX Sales`) and absolute low values on the visual, programmatically hiding intermediate label noise.

---

## 📐 Project Architecture & Dimensional Modeling

The data architecture strictly follows enterprise industry standards using the **Star Schema** methodology with 1-to-Many (1:N) relationships and unidirectional cross-filtering, keeping the semantic model exceptionally lightweight and high-performing:

* **Fact Tables:**
  * `tbl fSales_Historical`: Consolidated historical transactional data.
  * `tbl fSales_Predictions`: Machine Learning-generated output containing forecasted future series along with lower and upper error confidence bounds.
* **Dimension Tables:**
  * `pbi tbl dCalendar`: Unified time dimension serving as the temporal bridge between historical actuals and future predictions.
  * `tbl dStorages`: Enterprise dimension containing precise geospatial attributes (Cities and Provinces) mapping the international retail footprints.

---

## 📈 Strategic Engineering & DataViz Decisions

1. **Upstream Data Hygiene:** Chose to handle data engineering tasks—such as cleansing incomplete periods and multi-dimensional distribution logic—directly in the BigQuery SQL layer. This keeps the downstream `.pbix` model optimized by avoiding resource-heavy calculated columns.
2. **Spatial Distortion Mitigation in DataViz:** Deployed proportional sizing tree-maps coupled with bubble density maps. This design pattern prevents states with vast geographic areas but low sales volumes from dominating executive attention, guiding stakeholders directly to the actual *Share of Wallet* hot spots.
3. **Deployment Strategy & Governance:** Deployed the interactive dashboard to the cloud via Power BI Service using `Publish to Web`. Since the dataset contains no PII (Personally Identifiable Information) or sensitive commercial secrets, this approach optimizes portfolio friction by allowing recruiters immediate access without login walls, while demonstrating the underlying mechanics of Tenant Administration and Fabric/Power BI environment provisioning.

---

## 🔗 Live Interactive Demo

The interactive cloud executive dashboard can be accessed directly through the link below:

👉 **[Access the SUPERSTORE Interactive Dashboard Here]https://app.powerbi.com/view?r=eyJrIjoiZGU1NGZlOTctZjQzMC00ZWEyLThjODktOWFiMmU2YzQxZDBjIiwidCI6IjIzY2FlMWJkLThiNTQtNGI3ZC1iZWM4LTNlZGI0ZTUyNmU1YiJ9**

---
