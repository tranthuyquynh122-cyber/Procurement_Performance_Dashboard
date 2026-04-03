# 📊 How Efficient Is Our Procurement Process? – Procurement Performance Analysis | Power BI

**Business Question:**  
How can a company monitor purchasing activities, control total cost, and evaluate vendor performance to improve operational efficiency?

**Domain:** Supply Chain / Procurement  
**Tools Used:** Power BI  

---

## 📑 Table of Contents


1. 📌 [Background & Overview](#background--overview)
2. 📂 [Dataset Description & Data Structure](#dataset-description--data-structure)
3. 🧠 [Design Thinking Process](#design-thinking-process)
4. 📊 [Key Insights & Visualizations](#key-insights--visualizations)
5. 🔎 [Final Conclusion & Recommendations](#final-conclusion--recommendations)

---

<a id="background--overview"></a>
## 📌 Background & Overview  

### 🎯 Objective

### 📘 What is this project about?

This project focuses on analyzing **purchasing performance** using data from the AdventureWorks database.  
The goal is to help organizations better understand how procurement activities impact total cost, operational efficiency, and vendor performance.

This dashboard answers real-world business questions such as:

- How many purchase orders are being created over time?
- How much total cost is spent on procurement?
- Which vendors contribute the most to total cost?
- Are orders being delivered on time?
- Where do rejections or delays occur?
- Which categories or vendors drive inefficiencies?

The analysis transforms raw transactional data into clear, actionable insights that support smarter purchasing decisions.

---

### 👤 Who is this project for?

This project is designed for:

- Procurement Managers  
- Supply Chain Managers  
- Purchasing Officers  
- Operations Managers  
- Business Analysts  
- Decision-makers responsible for cost control and vendor performance  

---

<a id="dataset-description--data-structure"></a>
## 📂 Dataset Description & Data Structure  

### 📌 Data Source  

- Dataset: **AdventureWorks (Microsoft Sample Database)**  
- Domain: Manufacturing / Procurement  
- Data Type: Relational transactional data  
- Visualization Tool: Power BI  

The dataset contains purchasing transactions, vendor information, product details, and logistics attributes commonly found in real-world ERP systems.

---

## 📊 Tables Used in This Project  

Only tables relevant to purchasing analysis were selected.

---

## 🧾 Fact Tables  

### 📘 Fact.PurchaseOrderHeader  

<details>
<summary>Click to expand table schema</summary>

| Column Name | Data Type | Description |
|------------|-----------|-------------|
| PurchaseOrderID | INT | Unique identifier of each purchase order |
| RevisionNumber | TINYINT | Revision number of the order |
| Status | TINYINT | Order status (approved, completed, rejected, etc.) |
| EmployeeID | INT | Buyer responsible for the order |
| VendorID | INT | Vendor placing the order |
| ShipMethodID | INT | Shipping method used |
| OrderDate | DATETIME | Date the order was created |
| ShipDate | DATETIME | Date the order was shipped |
| SubTotal | MONEY | Total product value (excluding tax & freight) |
| TaxAmt | MONEY | Tax amount |
| Freight | MONEY | Shipping cost |
| TotalDue | MONEY | Total cost including tax and freight |
| ModifiedDate | DATETIME | Last update timestamp |

</details>

---

### 📘 Fact.PurchaseOrderDetail  

<details>
<summary>Click to expand table schema</summary>

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| PurchaseOrderDetailID | INT | Line-level identifier |
| PurchaseOrderID | INT | Reference to purchase order |
| DueDate | DATETIME | Expected delivery date |
| OrderQty | SMALLINT | Quantity ordered |
| ProductID | INT | Product identifier |
| UnitPrice | MONEY | Unit purchase price |
| LineTotal | MONEY | OrderQty × UnitPrice |
| ReceivedQty | DECIMAL | Quantity received |
| RejectedQty | DECIMAL | Quantity rejected |
| ModifiedDate | DATETIME | Last update timestamp |

</details>

---

## 🧱 Dimension Tables  

### 📘 Dim.Vendor  

<details>
<summary>Click to expand table schema</summary>

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| BusinessEntityID | INT | Vendor identifier |
| Name | NVARCHAR | Vendor name |
| CreditRating | TINYINT | Vendor credit rating |
| PreferredVendorStatus | BIT | Preferred vendor indicator |
| ActiveFlag | BIT | Vendor active status |
| PurchasingWebServiceURL | NVARCHAR | Vendor website |
| ModifiedDate | DATETIME | Last update timestamp |

</details>

---

### 📘 Dim.ProductVendor  

<details>
<summary>Click to expand table schema</summary>

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| ProductID | INT | Product identifier |
| BusinessEntityID | INT | Vendor identifier |
| AverageLeadTime | INT | Average delivery lead time (days) |
| StandardPrice | MONEY | Standard vendor price |
| LastReceiptCost | MONEY | Last received cost |
| LastReceiptDate | DATETIME | Last receipt date |
| MinOrderQty | INT | Minimum order quantity |
| MaxOrderQty | INT | Maximum order quantity |
| OnOrderQty | INT | Quantity currently on order |
| UnitMeasureCode | NCHAR(3) | Unit of measure |
| ModifiedDate | DATETIME | Last update timestamp |

</details>

---

### 📘 Dim.Product  

<details>
<summary>Click to expand table schema</summary>

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| ProductID | INT | Product identifier |
| Name | NVARCHAR | Product name |
| ProductNumber | NVARCHAR | Product code |
| StandardCost | MONEY | Standard manufacturing cost |
| ListPrice | MONEY | List price |
| ProductSubcategoryID | INT | Subcategory reference |
| SellStartDate | DATETIME | Start selling date |
| SellEndDate | DATETIME | End selling date |
| ModifiedDate | DATETIME | Last update timestamp |

</details>

---

### 📘 Dim.ProductSubcategory  

<details>
<summary>Click to expand table schema</summary>

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| ProductSubcategoryID | INT | Subcategory identifier |
| ProductCategoryID | INT | Category identifier |
| Name | NVARCHAR | Subcategory name |
| ModifiedDate | DATETIME | Last update timestamp |

</details>

---

### 📘 Dim.ShipMethod  

<details>
<summary>Click to expand table schema</summary>

| Column Name | Data Type | Description |
|-------------|-----------|-------------|
| ShipMethodID | INT | Shipping method identifier |
| Name | NVARCHAR | Shipping method name |
| ShipBase | MONEY | Base shipping cost |
| ShipRate | MONEY | Shipping rate |
| ModifiedDate | DATETIME | Last update timestamp |

</details>

---

## 🔗 Data Relationships  

<img width="659" height="697" alt="image" src="https://github.com/user-attachments/assets/75d36cc1-e55b-49d7-9349-1f4fb422adab" />


These relationships form a **Snowflake-schema–like model**, optimized for analytics and Power BI reporting.

---
<a id="design-thinking-process"></a>
## 🧠 Design Thinking Process  

### 1️⃣ Empathize  

**Primary stakeholder:** Procurement Manager  

**Pain points:**

- Difficult to track total purchasing cost across time  
- Limited visibility into vendor performance  
- Hard to identify delayed or rejected orders  
- Reports scattered across multiple sources  
- No centralized view for monitoring procurement health  

---

### 2️⃣ Define  

**Problem Statement**

> Procurement teams need a centralized dashboard to monitor **Total Cost** and **Total Orders** in order to control spending, identify inefficiencies, and improve supplier performance.

---

### 🎯 Key Metrics  

**Primary Metrics**

- **Total Cost**  
  Total financial amount spent on procurement, including product cost, tax, and freight.

- **Total Orders**  
  Total number of purchase orders created within a given period.  
  This metric reflects purchasing activity volume and workload.

**Supporting Metrics**

- Average Unit Price  
- Average Lead Time  
- Rejected Orders  
- On-time Delivery Rate  

---

### 3️⃣ Ideate  

The solution is structured into four analytical layers:

1. **Overview** – High-level KPIs and overall trends  
2. **Vendors** – Vendor performance and cost contribution  
3. **Orders** – Order status, rejection, and delivery analysis  
4. **Transactions** – Detailed drill-down for investigation and auditing  

Each layer answers progressively deeper questions:

> *What is happening → Why is it happening → Where should action be taken*

---

### 4️⃣ Prototype  

A Power BI dashboard was developed to:

- Present procurement KPIs in a clear and intuitive way  
- Enable filtering by time, vendor, category, and order status  
- Highlight inefficiencies and operational risks  
- Support data-driven decisio

<a id="key-insights--visualizations"></a>
## 📊 Key Insights & Visualizations

## 🔍 Dashboard Preview  

---

## 1️⃣ Dashboard 1 – Overview  

![image alt](https://github.com/tranthuyquynh122-cyber/Procurement_Performance_Dashboard/blob/ee8a6b236bdda5aa398eee31c71c39ab1949c18a/pc_overview.png)

This dashboard provides a high-level view of procurement performance, focusing on cost drivers, purchasing patterns, and supplier activity.

### 🔎 Observation

- Total procurement cost reaches **70.48M**, with **4,012 purchase orders**, indicating a high purchasing frequency.  
- Monthly cost fluctuates noticeably, with several peaks not accompanied by significant changes in unit price.  
- Average Unit Price (**~34.7**) remains relatively stable across time.  
- Average Lead Time (**~9 days**) shows limited variation.  
- Vendor Active Rate is relatively high (~96%), meaning most registered vendors are used.  
- Procurement spend is concentrated in a few product categories, particularly **Components and Bikes**.

---

### 💡 Insights

- Cost growth is primarily driven by **order volume rather than price increases**, indicating that procurement inefficiencies are more related to purchasing behavior than supplier pricing.  
- Stable pricing suggests supplier agreements are relatively controlled, and cost spikes are not driven by external price volatility.  
- High concentration in specific categories creates **exposure to category-level cost risk**, especially if demand or supply conditions change.  
- Stable lead time reflects operational consistency but may still hide opportunities for logistics optimization.  
- High vendor activity suggests a **broad vendor base that may not be fully optimized**, increasing management complexity without proportional value.

---

### ✅ Recommendations

- Review purchasing patterns to identify **unnecessary order fragmentation**, especially during peak months.  
- Introduce **order consolidation strategies** to reduce operational overhead and transaction costs.  
- Monitor high-spend categories (e.g., Components, Bikes) with **category-level cost control thresholds**.  
- Evaluate vendor utilization to identify **inactive or low-value vendors for consolidation**.  
- Use this dashboard as a **monthly monitoring tool** to track cost drivers and procurement efficiency.


---

## 2️⃣ Dashboard 2 – Vendors  

![image alt](https://github.com/tranthuyquynh122-cyber/Procurement_Performance_Dashboard/blob/ee8a6b236bdda5aa398eee31c71c39ab1949c18a/pc_vendors.png)

This dashboard analyzes supplier performance, cost distribution, and vendor-related risks in procurement operations.

### 🔎 Observation

- Out of **104 vendors**, only **86 are active**, while **18 remain inactive**.  
- A relatively small group of vendors accounts for a large portion of total procurement spend.  
- Several vendors with high total cost also show **higher rejection rates**.  
- Lead time varies significantly across vendors.  
- Some vendors are not marked as “Preferred” despite contributing substantial spend.  
- Certain subcategories show consistently higher average unit prices.

---

### 💡 Insights

- Procurement spend is **not evenly distributed**, indicating dependency on a limited number of key vendors, which increases supply risk.  
- Vendors with both **high cost and high rejection rates** directly reduce procurement efficiency and increase operational waste.  
- Lead time inconsistency suggests **uneven supplier reliability**, affecting planning and inventory management.  
- Misalignment between **Preferred Vendor status and actual spend** weakens vendor governance and decision-making.  
- Maintaining inactive vendors increases **data noise and administrative overhead** without contributing value.

---

### ✅ Recommendations

- Develop a **vendor performance framework** based on cost, lead time, and rejection rate.  
- Re-evaluate high-spend vendors with poor quality metrics to determine **contract renegotiation or replacement**.  
- Align **Preferred Vendor designation** with actual performance and contribution.  
- Remove or deactivate **inactive vendors** to streamline vendor management.  
- Investigate subcategories with high unit prices to identify **pricing inefficiencies or supplier issues**.

---

## 3️⃣ Dashboard 3 – Orders  

![image alt](https://github.com/tranthuyquynh122-cyber/Procurement_Performance_Dashboard/blob/ee8a6b236bdda5aa398eee31c71c39ab1949c18a/pc_orders.png)

This dashboard focuses on order execution, fulfillment performance, and logistics efficiency, highlighting how procurement activities are translated into operational outcomes.

### 🔎 Observation

- Out of **4,012 total orders**, the majority are completed, while a smaller portion is rejected or pending.  
- Rejected orders are not evenly distributed and tend to be concentrated among a subset of vendors.  
- Certain shipping methods consistently incur higher freight costs compared to others.  
- Order volume fluctuates across months, with noticeable variations in activity levels.  
- Some vendors consistently show higher rejection ratios than others.

---

### 💡 Insights

- Rejection patterns are **vendor-specific rather than random**, indicating that a small group of suppliers is responsible for a disproportionate share of order failures.  
- Vendors with repeated rejections are likely facing **ongoing issues in product quality, specification alignment, or fulfillment capability**, which can lead to recurring operational disruptions.  
- Higher freight costs associated with specific shipping methods suggest that **logistics decisions are not consistently optimized based on cost efficiency**, especially for certain order types.  
- Fluctuations in monthly order volume indicate **inconsistent procurement planning or unstable demand patterns**, which may reduce purchasing efficiency and increase operational complexity.  
- The combination of high rejection rates and high order frequency in certain vendors can create **compounding inefficiencies**, impacting both cost and service reliability.

---

### ✅ Recommendations

- Identify vendors with the **highest rejection frequency and volume impact**, and prioritize them for targeted performance reviews instead of applying broad supplier evaluations.  
- Reassess or renegotiate relationships with vendors that show **both high rejection rates and significant order volumes**, as they pose the greatest operational risk.  
- Analyze shipping methods associated with the **highest cost per order**, and standardize logistics selection to improve cost efficiency.  
- Improve procurement planning processes to **align purchasing frequency with demand patterns**, reducing unnecessary fluctuations in order volume.  
- Use this dashboard as a **weekly operational monitoring tool** to track vendor-level issues, detect recurring problems early, and support timely corrective actions.
---

## 4️⃣ Dashboard 4 – Transaction  

This dashboard provides detailed transaction-level visibility to support auditing and deep-dive analysis.

### 🔎 Observation

- The dashboard provides transaction-level visibility across procurement activities.  
- Most transactions are marked as **Completed**, with a smaller number marked as Rejected or Pending.  
- Certain vendors appear frequently across transactions, indicating repeated purchasing relationships.  
- Some transactions show **high total cost despite relatively low quantities**.  
- Large variations exist in unit price across transactions.

---

### 💡 Insights

- Transaction-level data reveals **cost drivers hidden behind aggregated KPIs**, enabling deeper investigation.  
- Frequent transactions with the same vendors indicate **dependency and repeated sourcing patterns**.  
- High-cost transactions with low quantities may signal **pricing anomalies or urgent procurement inefficiencies**.  
- Variability in unit price suggests **inconsistent pricing control or lack of standardization**.

---

### ✅ Recommendations

- Use transaction-level analysis to **audit unusual cost patterns and anomalies**.  
- Identify and review **outlier transactions** (high cost / low quantity).  
- Standardize pricing agreements to reduce **unit price variability**.  
- Leverage transaction history to support **vendor performance evaluation and negotiation**.  
- Enable drill-down usage for **procurement auditing and issue investigation**.
---

<a id="final-conclusion--recommendations"></a>
## 🔎 Final Conclusion & Recommendations 

### 📌 Final Conclusion 

- Procurement cost is primarily driven by **order volume rather than unit price changes**, indicating inefficiencies in purchasing frequency.  
- Spend is concentrated in both **specific categories and a limited number of vendors**, creating structural dependency risks.  
- Vendor performance varies significantly, particularly in **lead time and rejection rate**, affecting procurement efficiency.  
- Logistics and shipping method selection contribute to **avoidable cost increases**.  
- Transaction-level analysis highlights **hidden inefficiencies not visible in aggregated metrics**.

---

### 🎯 Recommendations

**1. Control purchasing frequency (High Priority)**  
Reduce unnecessary order fragmentation by consolidating purchases and improving planning cycles.

**2. Strengthen vendor governance (High Priority)**  
Implement a structured vendor evaluation model based on cost, lead time, and rejection rate.

**3. Rebalance vendor portfolio (Medium Priority)**  
Reduce dependency on high-risk vendors and align Preferred Vendor status with actual performance.

**4. Optimize logistics decisions (Medium Priority)**  
Review shipping method selection to minimize freight costs and improve cost efficiency.

**5. Leverage transaction-level monitoring (Ongoing)**  
Use detailed transaction data to detect anomalies, validate costs, and support procurement audits.


