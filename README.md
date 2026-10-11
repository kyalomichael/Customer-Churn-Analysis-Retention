# Customer Churn Prediction & Retention

An end-to-end executive Power BI analytics platform designed to uncover primary revenue leaks, technical support gaps, customer retention risks, and operational churn drivers for a telecommunications ecosystem.

---

## 📊 Executive Summary
This 3-page executive diagnostic dashboard translates raw telecommunications subscriber data into strategic retention playbooks. Built on a clean star-schema model, the platform analyzes churn patterns across 7,043 customer accounts, isolating key behavioral drivers and revenue risks across contractual tiers and technical support services.

### Key Performance Highlights
- **Total Customer Accounts:** 7,043 subscriber profiles analyzed.
- **Overall Churn Rate:** 26.5% overall customer attrition rate.
- **Critical Vulnerability Window:** ~67% of annual churn occurs within the first 12 months of onboarding.
- **Primary Churn Spike:** Fiber Optic subscribers on Month-to-Month contracts exhibit a 54.6% churn rate.
- **Service Gap Impact:** Customers lacking technical support add-ons churn at nearly 3x the rate of enrolled users.

---

## 🏗️ Architecture & Pipeline Phases

### Pipeline Architecture
- **Data Preprocessing & EDA:** Python (`pandas`, `numpy`) & Jupyter Notebooks
- **Database Engine:** Relational SQL schema design (`churn_analysis_queries.sql`)
- **Data Pipeline:** Python ETL / SQL queries ingested into Power BI Desktop
- **Data Schema:** Star Schema centered on customer service and subscription facts

### Pipeline Phases
* **Phase 1: Data Preprocessing & Hygiene (`cleaned_telco_churn.csv`)** — Resolved missing value anomalies in high-impact columns (such as `TotalCharges` for fresh sign-ups), standardized data types, and mapped categorical text into uniform flags.
* **Phase 2: Exploratory Data Analysis (`phase2_statistical_eda.ipynb`)** — Investigated demographic drivers, payment frictions, and service engagement patterns to map customer vulnerability windows.
* **Phase 3: Relational SQL Modeling (`churn_analysis_queries.sql`)** — Designed logical schema structures and executed conditional aggregations, window functions, and multi-table joins to isolate high-risk customer segments prior to visualization.
* **Phase 4: Power BI Modeling & DAX Engine** — Configured star-schema relationships and engineered calculated virtual summary tables (`Service_Risk_Summary`) using `UNION` and `SELECTCOLUMNS` for cross-service evaluation without circular dependencies.
* **Phase 5: Executive UI/UX Design** — Built a modern, 3-page interactive report adhering to Fluent2 standards with custom card containers, off-white canvas (`#F8F9FA`), deep indigo accents (`#1A1961`), and vertical category side banners.

### Star Schema Relationships
- **`fact_churn`** ↔ **`dim_customers`** (1:* via `CustomerID`)
- **`fact_churn`** ↔ **`dim_services`** (1:* via `ServiceID`)
- **`fact_churn`** ↔ **`dim_contracts`** (1:* via `ContractID`)
- **`Service_Risk_Summary`**: Custom calculated DAX virtual table for cross-service evaluation.

---

## 🖥️ Dashboard Preview & Structure

### Page 1: Executive Overview
High-level KPI cards (Total Customers, Overall Churn Rate, Average Monthly Charges, Tech Support Deficit Rate), monthly subscription trajectory charts, and overall financial exposure across tenure cohorts.

![Executive Overview Dashboard](screenshots/page1_executive_summary.png)

---

### Page 2: Root Cause & Driver Analysis
Monthly charge tier distributions, side-by-side clustered bar matrices comparing technical support add-ons (`Tech Support`, `Online Security`, `Online Backup`, `Device Protection`), and contract cross-segmentation isolating Fiber Optic vs. DSL vulnerabilities.

![Root Cause Analysis Dashboard](screenshots/page2_root_cause.png)

---

### Page 3: Risk Segmentation & Action Plan
Interactive customer risk matrix categorizing subscriber profiles into high, medium, and low churn tiers, accompanied by targeted cohort action lists for immediate retention outreach and VIP discount offers.

![Risk Segmentation Dashboard](screenshots/page3_risk_segmentation.png)


## 📈 Key Diagnostic Findings & Churn Triggers
* **Fiber Optic + M2M Vulnerability**: The combination of Fiber Optic internet and Month-to-Month contracts drives churn rates as high as **54.6%**, pointing to a lack of long-term commitment incentives.
* **The Technical Support Deficit**: Customers without technical support add-ons churn at nearly **3x the rate** of those enrolled in support tiers.
* **Onboarding Window**: Approximately **67% of total annual churn** occurs within the first 12 months of customer tenure.
* **Payment Friction**: Electronic check users suffer disproportionately higher churn rates (~45.2%), signaling manual billing friction or processing drop-offs.

---

## ⚙️ Core DAX Measures

```dax
// Total Churned Customers
Total Churned Customers = CALCULATE(COUNT(fact_churn[CustomerID]), fact_churn[Churn_Flag] = 1)

// Overall Churn Rate
Overall Churn Rate = DIVIDE([Total Churned Customers], COUNT(fact_churn[CustomerID]), 0)

// Service Churn Rate
Service Churn Rate = AVERAGE(Service_Risk_Summary[Churn_Flag])

// Support Deficit Benchmark
Support Deficit Rate = 0.285