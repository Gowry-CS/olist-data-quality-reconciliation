<div align="center">
<h2>Olist Data Reconciliation (Silver & Gold Lakehouse Validation)</h2> 
</div>

### 📌 Project Overview

This project focuses on **validating the integrity, completeness, and consistency** of curated Silver- and Gold-layer e-commerce data in a Medallion Lakehouse.  

The goal is to ensure that data moving from **Bronze → Silver → Gold** is **accurate, reliable, and trustworthy** for downstream analytics and reporting.

Validation was performed across **all core data quality dimensions**, supporting enterprise-grade analytics and executive dashboards.

### 🥈 Silver Lakehouse Validation

The Silver layer focuses on **data quality, consistency, and integrity** after cleaning and standardisation.

| Data Quality Dimension | Purpose | Specified Checks |
|------------------------|---------|-----------------|
| **Completeness** | Are all required data values present? (Focus on Nulls) | Verified no nulls in all mandatory ID fields (`order_id`, `customer_id`, `product_id`, `seller_id`). |
| **Validity** | Does the data conform to defined schema, format, or range? (Focus on Schema & Outliers) | Enforced consistent data types: IDs as `STRING`, dates as `DATE/TIMESTAMP`, monetary fields as `INT/FLOAT`. Checked for invalid values in critical fields (non-negative prices/freight, review scores 1–5). |
| **Consistency** | Is the data logical and aligned across tables? (Focus on Dates & Foreign Keys) | Validated referential integrity (FKs). Confirmed logical date sequences (purchase → approval → shipment → delivery). Only 1 record was found not to have a logical date sequence. No further changes were made and it was excluded from analysis. |
| **Uniqueness** | Are there duplicate records where they should not exist? (Focus on PKs & duplicates) | Enforced uniqueness in all Dimension tables (`customers`, `sellers`, `products`). Handled known duplication issues in primary identifiers (`order_id` & `review_id`). |
| **Integrity** | Does the data comply with core business rules? (Focus on Business Logic) | Verified financial integrity by ensuring total payments match the calculated order total (items + freight). Enforced ≥1 item and ≥1 payment per order. |

#### 🔑 Comments on Primary Keys

- **Order ID (`order_id`)**: Duplication is expected due to one-to-many relationships with `order_items`. It is **not used as a primary key** in the fact table.  
- **Review ID (`review_id`)**: 66 true duplicates were found. These records were **removed** to ensure a clean dimension table.

### 🏆 Gold Lakehouse Validation

Validation of the Gold-layer Galaxy Schema ensures that **analytics-ready datasets are accurate, complete, and performant** for BI and downstream reporting.

| Data Quality Dimension | Check (What is Verified) | Why It’s Critical (Impact on Downstream Users) |
|------------------------|-------------------------|-----------------------------------------------|
| **Integrity (Galaxy Schema)** | Referential Integrity: Every fact table Foreign Key (e.g., `customer_id`, `date_id`, `product_id`) exists in the corresponding dimension table | Guarantees reliable joins and prevents broken relationships or orphaned fact records in BI tools |
| **Completeness** | Mandatory Key Check: Verify that no surrogate keys (e.g., `date_id`, `customer_id`) are NULL in the final fact tables | Ensures all fact records can successfully connect to their dimension attributes, preventing data loss during analysis *Exception: 8 records where only `order_id` is available* |
| **Accuracy (Reconciliation)** | Metric Alignment: Verify that aggregated metrics (e.g., average review scores in `fact_reviews`) match values in `fact_orders` table | Ensures financial and operational measures are consistent across all views, preserving trust in core business metrics |
| **Performance (Usability)** | Query Efficiency: Validate that Star Schema joins between Facts and Dimensions complete within acceptable time thresholds | Guarantees timely access to data, keeping dashboards and reporting efficient and user-friendly |

### 💡 Key Takeaways

- Gold-layer validation **confirms the analytical readiness** of datasets for executive dashboards and decision-making.  
- Referential integrity and completeness checks **prevent broken joins and missing data**, avoiding misleading insights.  
- Metric reconciliation ensures **trustworthy KPIs** for stakeholders.  
- Performance checks guarantee that **large-scale joins are efficient**, supporting scalable BI workloads.

### 🛠️ Tools & Skills Demonstrated

- **PySpark**: Data validation, type enforcement, null handling, duplicate detection  
- **Spark SQL**: Referential integrity checks, foreign key validation, aggregation validation  
- **Microsoft Fabric**: Lakehouse implementation and Medallion Architecture validation  
- **Data Quality Engineering**: Completeness, validity, consistency, uniqueness, and integrity checks  
- **Analytical Thinking**: Detecting data anomalies, enforcing business rules, and preserving trustworthy datasets  
- **Data Modelling**: Galaxy schema design for OLAP optimisation

### 🎯 Business Impact & Outcomes

- Ensured **trusted Silver- and Gold-layer datasets** for downstream analytics and dashboards  
- Prevented inaccurate reporting due to missing, inconsistent, or duplicate records  
- Established a **reliable foundation** for vendor performance and sales analytics  
- Improved confidence for leadership in **decision-making based on validated data**  
- Enabled scalable BI workloads and faster insights for executives

