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

---

### 📌 Analysis 1

### 🔎 Observation

- **Total Cost reaches approximately 70.48M**, indicating a significant procurement spend that requires close monitoring.
- **Total Orders = 4,012**, showing a high volume of purchasing transactions during the analyzed period.
- Monthly trends show noticeable fluctuations in total cost, suggesting **seasonality or irregular purchasing behavior**.
- **Average Unit Price (~34.7)** remains relatively stable across time, indicating price consistency from suppliers.
- **Average Lead Time (~9 days)** is fairly stable, showing controlled delivery performance.
- The **Vendor Active Rate (~96%)** indicates that most registered vendors are actively used.
- Spending is highly concentrated in a few major product categories (e.g., Components and Bikes).

---

### 💡 Insights

- Changes in **Total Cost are mainly driven by the number of orders**, rather than price increases.
- Stable unit prices suggest that procurement pricing agreements are relatively well controlled.
- High dependence on a few major categories increases exposure to cost concentration risk.
- Stable lead time reflects a mature logistics process, but still leaves room for optimization.
- High vendor activity may create operational complexity if not properly governed.

---

### ✅ Recommendations

- Focus on **controlling order volume**, especially during peak months with cost spikes.
- Introduce order consolidation policies to reduce unnecessary purchasing frequency.
- Set cost monitoring thresholds for high-spend categories.
- Continuously monitor lead time trends to detect early supply chain disruptions.
- Use this dashboard as a monthly executive overview for procurement performance tracking.

---

## 2️⃣ Dashboard 2 – Vendors  

![image alt](https://github.com/tranthuyquynh122-cyber/Procurement_Performance_Dashboard/blob/ee8a6b236bdda5aa398eee31c71c39ab1949c18a/pc_vendors.png)

---

### 📌 Analysis 2

### 🔎 Observation

- The system contains **104 vendors**, of which **86 are active** and **18 inactive**.
- A small group of vendors contributes a disproportionately large share of **Total Cost**.
- Some vendors with high spending also show **higher rejection rates**.
- **Average lead time varies significantly** across vendors.
- Several vendors are not marked as *Preferred* but still account for notable spending.
- Certain subcategories show unusually high average prices.

---

### 💡 Insights

- Procurement spending is concentrated among a limited number of vendors, creating dependency risk.
- High-cost vendors with elevated rejection rates indicate potential quality or compliance issues.
- Lead time variability suggests inconsistent operational performance across suppliers.
- Maintaining inactive or low-value vendors increases data noise and management complexity.
- Lack of alignment between vendor status and actual spending reduces procurement governance effectiveness.

---

### ✅ Recommendations

- Build a **vendor performance scorecard** using cost, lead time, and rejection rate.
- Prioritize strategic partnerships with high-performing vendors.
- Review and renegotiate contracts with vendors that have high cost but poor performance.
- Reduce reliance on vendors with long lead times or high rejection ratios.
- Clean and maintain the vendor master list to remove inactive or unused suppliers.
- Enforce clearer rules for assigning *Preferred Vendor* status.

---

## 3️⃣ Dashboard 3 – Orders  

![image alt](https://github.com/tranthuyquynh122-cyber/Procurement_Performance_Dashboard/blob/ee8a6b236bdda5aa398eee31c71c39ab1949c18a/pc_orders.png)

---

### 📌 Analysis 3

### 🔎 Observation

- Out of **4,012 total orders**, most are completed successfully, while a smaller portion are rejected or pending.
- Rejected orders tend to be concentrated among specific vendors.
- Certain shipping methods generate significantly higher freight costs.
- Monthly order volume fluctuates, with noticeable drops in some periods.
- Some vendors consistently show higher rejection ratios than others.

---

### 💡 Insights

- Rejected orders indicate issues related to quality, specification mismatch, or fulfillment problems.
- High freight cost shipping methods may inflate total procurement cost without proportional benefits.
- Order volume volatility suggests weaknesses in demand planning or procurement scheduling.
- Repeated rejections from the same vendors signal systemic supplier performance issues.

---

### ✅ Recommendations

- Perform root-cause analysis on rejected orders by vendor and product category.
- Define and monitor **rejection rate KPIs** for each supplier.
- Review shipping method policies to reduce unnecessary logistics expenses.
- Improve coordination between planning and procurement teams to stabilize order volumes.
- Use this dashboard as a daily or weekly monitoring tool for operational control.

---
## 4️⃣ Dashboard 4 – Transaction  

👉 *Insert Power BI dashboard screenshot here*

---

### 📌 Analysis 4
![image alt](https://github.com/tranthuyquynh122-cyber/Procurement_Performance_Dashboard/blob/ee8a6b236bdda5aa398eee31c71c39ab1949c18a/pc_transaction.png)

### 🔎 Observation

- The dashboard provides a **transaction-level view** of purchasing activity, displaying detailed information such as:
  - Purchase Order ID  
  - Order Date  
  - Vendor  
  - Item Category  
  - Unit Price  
  - Order Quantity  
  - Cost Price  
  - Due Date  
  - Total Due  
  - Order Status  

- Most transactions are marked as **Completed**, while a smaller portion remains **Rejected** or **Pending**.
- Several vendors appear repeatedly across multiple transactions, indicating **frequent purchasing relationships**.
- There is a wide variation in **unit price and order quantity**, leading to significant differences in total transaction value.
- Some transactions show **high total cost even with small quantities**, which may require further validation.
- Filters by year/month, vendor, category, and order status allow users to drill down into specific transactions easily.

---

### 💡 Insights

- Transaction-level visibility enables detailed investigation of cost drivers behind aggregated KPIs.
- Vendors with frequent transactions may represent key suppliers or potential dependency risks.
- Rejected transactions highlight potential issues related to quality, documentation, or fulfillment.
- High-value transactions deserve closer monitoring to avoid budget overruns or approval issues.
- The ability to search by Purchase Order ID supports operational auditing and issue resolution.

---

### ✅ Recommendations

- Use the transaction table as an **audit-level control tool** to investigate anomalies detected at summary dashboards.
- Regularly review rejected transactions to identify root causes and prevent recurrence.
- Monitor high-value transactions and introduce approval thresholds if necessary.
- Consolidate small, repetitive orders to reduce administrative workload and processing costs.
- Leverage vendor-level transaction patterns to support supplier performance evaluations.

---

<a id="final-conclusion--recommendations"></a>
## 🔎 Final Conclusion & Recommendations  

👉 Based on the overall findings, the following strategic actions are recommended for procurement stakeholders:

### ✅ Key Takeaways

- **Total Cost is primarily driven by the number of orders**, not by unit price increases.
- Procurement spending is highly concentrated among a small group of vendors.
- Vendor performance varies significantly in terms of lead time and rejection rate.
- Inefficient shipping methods and rejected orders contribute to unnecessary cost.
- A centralized dashboard enables early detection of risks and inefficiencies.

### 🎯 Recommendations

- Strengthen governance over order creation and reduce excessive order volume.
- Establish a standardized vendor performance evaluation framework.
- Prioritize high-performing vendors and reconsider relationships with low-performing ones.
- Optimize logistics and shipping method selection to reduce freight costs.
- Use the dashboard continuously as a decision-support tool for procurement leadership.



