# Expiry Risk & Capital Exposure Analysis: Pharmaceutical Supply Chain

## 📌 Project Overview
This project analyzes over 35,000 records of pharmaceutical inventory to identify financial leakage, mitigate compliance risks, and optimize capital allocation. Using PostgreSQL, I built analytical models to track expiry proximity, quantify dead stock, and assess risk exposure across the product line. 

# 🏥 Pharmaceutical Expiry Risk & Capital Exposure Audit
### **Optimizing Inventory Turnover & Mitigating Financial Leakage (35,000+ Records)**

## 📊 Executive Summary
In the high-stakes pharmaceutical supply chain, stagnant inventory isn't just a storage issue—it’s a regulatory hazard and a severe financial liability. This project involves a comprehensive audit of over 35,000 global inventory records to identify margin leakage, quantify "Zombie" (dead) stock, and mitigate catastrophic expiry risks.

By engineering a series of PostgreSQL models, I transformed raw transactional data into **Board-Level Financial Insights**, focusing on three operational pillars: **Capital Recovery, Risk Mitigation, and Operational Efficiency**.

---

## 🛠️ Technical Arsenal
* **Database Engine:** PostgreSQL
* **Core Techniques Applied:**
  * **Window Functions (`OVER()`, `SUM()`):** Engineered for real-time risk contribution and percentage exposure analysis.
  * **Common Table Expressions (CTEs):** Utilized for multi-stage financial post-mortems and expiry exposure calculations.
  * **Conditional Logic (`CASE` Statements):** Built an automated Expiry Proximity Model to categorize risk buckets.
  * **Data Quality Assurance (QA):** Implemented programmatic NULL-handling and batch-integrity checks prior to analysis.

---

## 🔍 The 12-Point Strategic Audit & Financial Findings

### Phase 1: Capital Overview & Baseline Visibility
**1. Absolute Capital Lockup**
* **Insight:** Established the baseline financial exposure by calculating the total inventory value across all warehouses `SUM(Unit_Cost × Quantity_On_Hand)`.
* **Impact:** Provides executives with the exact dollar amount of working capital currently sitting in storage.

**2. Product Value Concentration (Top 10 Risk)**
* **Insight:** Identified the Top 10 most expensive products by aggregating inventory value per product. 
* **Impact:** Highlights where security, climate-control, and sales efforts must be disproportionately focused to protect high-tier investments.

**3. Data Integrity & Compliance Audit**
* **Insight:** Audited 35,000+ rows for missing `Batch_ID` and `Exp_Date`.
* **Impact:** Reduced regulatory risk by flagging undocumented batches that could lead to severe compliance penalties or forced recalls.

### Phase 2: Expiry Risk & Operational Leakage
**4. The 90-Day Expiry "Cash Trap" (12.79% Exposure)**
* **Insight:** The Expiry Exposure Model (built via CTEs) revealed that **12.79%** of total warehouse capital is locked in inventory expiring within the next 90 days.
* **Impact:** Represents an immediate, severe threat to operating cash flow, requiring urgent B2B discounting protocols.

**5. Automated Expiry Proximity Categorization**
* **Insight:** Developed a dynamic `CASE` statement model to categorize inventory into action-oriented buckets: Critical Risk (<=30 days), High Risk (31-90 days), Medium Risk (91-180 days), and Safe (>180 days).
* **Impact:** Replaces manual tracking with an automated triage system for the warehouse floor.

**6. Product-Level Risk Contribution (%)**
* **Insight:** Utilized advanced Window Functions to calculate the exact percentage of total risk value attributed to each specific product within the 90-day danger zone.
* **Impact:** Allows procurement to pinpoint exactly which SKUs are driving the highest proportional risk.

### Phase 3: Post-Mortem & Dead Stock Isolation
**7. "Zombie" Stock (Dead Inventory) Isolation**
* **Insight:** Isolated items physically unmoved in the warehouse for over 365 days with zero recent sales volume.
* **Finding:** The model revealed that **DermaLux_458** alone accounts for **$172.3 Million** in locked capital sitting physically unmoved for over a year.

**8. Historical Revenue Post-Mortem (The Cash Bleed)**
* **Insight:** Analyzed previously expired batches to calculate "Total Hard Loss" vs. "Total Lost Revenue".
* **Finding:** Identified **GlucoBalance_494** as the worst historical offender, resulting in over **7cr** in lost revenue opportunity.

**9. Slow-Moving, High-Value Risk Detection**
* **Insight:** Flagged SKUs possessing a monthly sales volume under 5 units, but holding over 200 units in stock at a high unit cost.
* **Impact:** Identifies "future dead stock" before it crosses the 365-day threshold, allowing for proactive liquidation.

### Phase 4: Advanced Operational Analytics
**10. Warehouse Risk Ranking & Composite Scoring**
* **Insight:** Created a composite risk score for inventory (+3 for expiry < 90 days, +4 for non-compliant, etc.) to rank total risk exposure per warehouse.
* **Impact:** Directs regional managers to the exact facilities requiring immediate operational intervention.

**11. Cold-Chain Integrity & Inspection Failures**
* **Insight:** Segmented inspection failure rates by temperature bands (e.g., 2–8°C vs 16–25°C) to compare warehouse cold-chain reliability.
* **Impact:** Isolates infrastructure failures that are ruining stock prior to expiration.

**12. Discount Effectiveness & Turnover Simulation**
* **Insight:** Modeled the correlation between high discount rates and actual sales movement (Turnover Ratio).
* **Impact:** Proves whether current pricing and discount strategies are actually successfully moving high-risk stock, or if they are failing.

---

## 💻 Core SQL Architecture Highlights

### I. The Expiry Exposure Model (CTE)
Calculates the exact percentage of total capital at risk within 90 days.
```sql
WITH total_inventory AS (
    SELECT SUM(qty_on_hand * unit_cost) AS total_value
    FROM fact_pharma_inventory
),
expiry_90_days AS (
    SELECT SUM(qty_on_hand * unit_cost) AS expiring_value
    FROM fact_pharma_inventory
    WHERE exp_date >= CURRENT_DATE + INTERVAL '90 days'
)
SELECT 
    e.expiring_value, t.total_value,
    ROUND((e.expiring_value / t.total_value) * 100, 2) AS expiry_exposure_percent
FROM total_inventory t CROSS JOIN expiry_90_days e;
```

 II. The Dead Stock Targeter
Isolates massive capital inefficiencies to trigger immediate write-offs or liquidation.
```sql
SELECT
    PRODUCT_NAME,
    COUNT(BATCH_ID) AS DEAD_BATCHES,
    SUM(QTY_ON_HAND * UNIT_COST) AS TOTAL_CAPITAL_LOCKED_DEADSTOCK
FROM FACT_PHARMA_INVENTORY
WHERE QTY_ON_HAND > 0 AND (CURRENT_DATE - MFG_DATE > 365)
GROUP BY PRODUCT_NAME
ORDER BY TOTAL_CAPITAL_LOCKED_DEADSTOCK DESC
LIMIT 10;
```
### 📈 Executive Recommendations
Immediate Liquidation Protocol: Target "Critical Risk" buckets (0-30 days) and the $172.3M DermaLux_458 dead stock for immediate B2B bulk discounting to recover partial capital.

Procurement Halt: Enforce a hard stop on procurement for GlucoBalance_494 until the historical 7cr cash bleed is stabilized and inventory turnover ratios improve.

Automated Triage Integration: Integrate the RISK_BUCKETS query into the daily warehouse management system (WMS) to shift picking strategy from FIFO (First-In-First-Out) to FEFO (First-Expired-First-Out).


Role: Data Analyst | Inventory & Operational Efficiency

## 💻 Master SQL Script
All queries used to generate these insights can be found in the [`PHARMA.sql`](PHARMA.sql) file.


















