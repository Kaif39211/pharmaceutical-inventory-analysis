# Expiry Risk & Capital Exposure Analysis: Pharmaceutical Supply Chain

## 📌 Project Overview
This project analyzes over 35,000 records of pharmaceutical inventory to identify financial leakage, mitigate compliance risks, and optimize capital allocation. Using PostgreSQL, I built analytical models to track expiry proximity, quantify dead stock, and assess risk exposure across the product line. 

## 🛠️ Tech Stack & Skills
* **Database:** PostgreSQL
* **Techniques:** Advanced SQL (CTEs, Window Functions, Subqueries), Data Aggregation, `CASE` statements for risk modeling, Financial impact calculations.

## 📂 Dataset
The dataset contains 35,000+ rows of pharmaceutical inventory data, including:
* Batch IDs and Product Names
* Manufacturing and Expiration Dates
* Quantity on Hand
* MRP and Unit Costs

## 🎯 Key Business Problems Addressed

* **Insight 1: The 90-Day Expiry Crisis**
  Discovered a catastrophic risk exposure: **12.79%** of the total warehouse capital is locked in inventory expiring within the next 90 days. This represents an immediate, severe threat to operating cash flow. 

* **Insight 2: The Historical Cash Bleed**
  Identified `GlucoBalance_494` as the worst historical offender for expired inventory. This single product line has already cost the company  in sunk hard costs, and over **7cr** in lost revenue opportunity.

* **Insight 3: The Zombie Inventory**
  Isolated massive capital inefficiencies in >365-day dead stock. The model revealed that `DermaLux_458` alone accounts for **$172.3 Million** in locked capital sitting physically unmoved in the warehouse for over a year.

## 💡 Actionable Insights (Query Results)
Insight 1: The 90-Day Expiry Crisis. Discovered a catastrophic risk exposure: 12.79% of the total warehouse capital is locked in inventory expiring within the next 90 days. This represents an immediate, severe threat to operating cash flow. 

Insight 2: The Historical Cash Bleed. Identified GlucoBalance_494 as the worst historical offender for expired inventory.

Insight 3: The Zombie Inventory. Isolated massive capital inefficiencies in >365-day dead stock. The model revealed that DermaLux_458 alone accounts for $172.3 Million in locked capital sitting physically unmoved in the warehouse for over a year. 

## 💻 Master SQL Script
All queries used to generate these insights can be found in the [`PHARMA.sql`](PHARMA.sql) file.
