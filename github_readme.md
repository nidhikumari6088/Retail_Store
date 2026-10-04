# 📊 Retail Analytics & Customer Behavior Optimization (SQL)

![Database](https://img.shields.io/badge/Database-MySQL%20%7C%20PostgreSQL-blue?style=flat-square&logo=mysql)
![Category](https://img.shields.io/badge/Domain-Retail%20Analytics-green?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-orange?style=flat-square)

An end-to-end SQL analytics project focused on resolving database inconsistencies, processing raw retail transactional logs, and extracting key business insights such as **Month-over-Month (MoM) revenue growth**, **product performance metrics**, and **customer behavioral segmentation**.

---

## 📑 Table of Contents
- [Business Problem](#-business-problem)
- [Project Architecture & Schema](#-project-architecture--schema)
- [Key Features & Analytics Workflow](#-key-features--analytics-workflow)
  - [1. Data Cleaning & Schema Standardization](#1-data-cleaning--schema-standardization)
  - [2. Sales & Inventory Performance](#2-sales--inventory-performance)
  - [3. Customer Behavior & Loyalty Metrics](#3-customer-behavior--loyalty-metrics)
- [Getting Started / Setup Instructions](#-getting-started--setup-instructions)
- [Repository Structure](#-repository-structure)
- [Insights & Business Recommendations](#-insights--business-recommendations)

---

## 📌 Business Problem

Retail businesses often face challenges with fragmented data sources, pricing discrepancies across inventory and sales systems, missing customer metadata, and a lack of granular buyer segmentation. 

This project addresses these operational issues by leveraging relational SQL queries to:
1. Deduplicate transaction logs and reconcile mismatched pricing across tables.
2. Standardize text-formatted timestamp columns for time-series analysis.
3. Compute business metrics including sales growth percentage, customer purchasing frequency, and product tiering.

---

## 🗄️ Project Architecture & Schema

The relational setup relies on three core entities:

```
                  +-----------------------+
                  |   customer_profiles   |
                  +-----------------------+
                  | CustomerID (PK)       |
                  | Age                   |
                  | Gender                |
                  | Location              |
                  | JoinDate              |
                  +-----------+-----------+
                              |
                              | 1:N
                              v
+------------------------+   +------------------------+
|   product_inventory    |   |   sales_transaction    |
+------------------------+   +------------------------+
| ProductID (PK)         |<--| TransactionID (PK)     |
| Category               | N:1| CustomerID (FK)        |
| Price                  |   | ProductID (FK)         |
+------------------------+   | QuantityPurchased      |
                             | TransactionDate        |
                             | Price                  |
                             +------------------------+
```

---

## 🛠️️ Key Features & Analytics Workflow

### 1. Data Cleaning & Schema Standardization

* **Deduplication:** Identified duplicated transaction keys and migrated unique records into a clean production table structure.
* **Pricing Synchronization:** Identified and fixed discrepancies where transaction prices differed from master inventory records.
* **Data Imputation & Type Casting:** Processed missing fields in customer records using `COALESCE()` and transformed string-based dates (`TEXT`) into native SQL `DATE` data types.

### 2. Sales & Inventory Performance

* **Revenue Drivers:** Ranked Top 10 revenue-generating products and flagged bottom-tier items with low sales velocity.
* **Category Revenue:** Aggregated total volume sold and total revenue per product line.
* **Time-Series MoM Analysis:** Utilized SQL Window Functions (`LAG() OVER()`) to evaluate Month-over-Month (MoM) revenue trajectory:

$$\text{MoM Growth \%} = \frac{\text{Sales}_{\text{Current}} - \text{Sales}_{\text{Previous}}}{\text{Sales}_{\text{Previous}}} \times 100$$

### 3. Customer Behavior & Loyalty Metrics

* **Customer RFM/Segmentation:** Built volume-based behavioral buckets using conditional logic:
  * **Low Tier:** $\le 10$ items purchased
  * **Mid Tier:** $11 - 30$ items purchased
  * **High-Value Tier:** $> 30$ items purchased
* **Customer Lifetime Span:** Computed retention active duration using `DATEDIFF(MAX(date), MIN(date))`.
* **Repeat Buyer Analysis:** Identified customers purchasing identical product SKUs on multiple distinct trips.

---

## 🚀 Getting Started / Setup Instructions

### Prerequisites
* **SQL Database Engine:** MySQL 8.0+, PostgreSQL 13+, or SQLite 3.x
* **Database Client:** DBeaver, MySQL Workbench, pgAdmin, or VS Code SQL tools

### Step-by-Step Installation

1. **Clone the Repository**
   ```bash
   git clone https://github.com/your-username/retail-analytics-sql.git
   cd retail-analytics-sql
   ```

2. **Initialize Database & Schema**
   Import the dataset or run the schema script into your database client:
   ```sql
   CREATE DATABASE retail_analytics;
   USE retail_analytics;
   ```

3. **Execute Analysis Scripts**
   Run the scripts in sequential order:
   * `01_data_cleaning.sql`: Removes duplicates, standardizes formats, and resolves price mismatches.
   * `02_sales_analysis.sql`: Performs aggregated sales, product performance, and MoM analysis.
   * `03_customer_segmentation.sql`: Executes behavioral segmentation and retention duration queries.

---

## 📂 Repository Structure

```
retail-analytics-sql/
├── data/
│   ├── raw_sales_transaction.csv
│   ├── product_inventory.csv
│   └── customer_profiles.csv
├── sql_scripts/
│   ├── 01_data_cleaning.sql
│   ├── 02_sales_analysis.sql
│   └── 03_customer_segmentation.sql
├── docs/
│   └── database_schema.png
├── .gitignore
├── LICENSE
└── README.md
```

---

## 📈 Insights & Business Recommendations

1. **Data Governance First:** Implementing foreign key constraints and automated update triggers will prevent price drift between inventory tables and sales logs.
2. **Promotional Focus:** Category aggregations indicate that high-performing products should be prioritized in targeted ad spend, while bottom-performing items require stock rationalization.
3. **Targeted Campaigns:** High-Value segment customers (>30 units) contribute disproportionately to total volume—loyalty rewards programs should be customized specifically for this cohort.

---

## 📄 License
This project is open-source and available under the [MIT License](LICENSE).