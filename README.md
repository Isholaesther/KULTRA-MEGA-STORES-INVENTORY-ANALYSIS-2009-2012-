# 📊 Kultra Mega Stores Inventory Analysis (2009–2012)

A Business Intelligence project analyzing sales, customer behavior, and shipping patterns for Kultra Mega Stores (KMS) to provide actionable insights to improve business operations.

## Table of Contents
1. [Project Overview](#project-overview)  
2. [Data Source](#data-source)  
3. [Tools and Technique](#tools-and-technique)  
4. [Exploratory Data Analysis](#exploratory-data-analysis)  
5. [Observations and Insights](#observations-and-insights)  
6. [Recommendations](#recommendations)  
7. [Limitations](#limitations)  

---

## 📁 Project Overview
Kultra Mega Stores (KMS), headquartered in Lagos, specializes in office supplies and furniture. Their customers include individual consumers, small businesses, and corporate clients across Nigeria.  
As a Business Intelligence Analyst, I was tasked to analyze KMS's Abuja division order data (2009–2012) to uncover key business insights and customer behaviors using SQL.

---

## 📂 Data Source
The dataset used includes:
- `KMS`: Sales and customer information (product category, sub-category, region, segment, etc.)
- `Order_Status`: Returns data
  

---

## 🛠️ Tools and Technique
- **SQL Server 2022** for querying and analysis  
- SQL joins, aggregate functions (`SUM`, `COUNT`, `AVG`)  
- Common Table Expressions (CTEs) for filtering and ranking  

---

## 🔍 Exploratory Data Analysis

### Case Scenario I

**1. Product category with highest sales**
```sql
SELECT TOP 1 Product_Category, SUM(Sales) AS TotalSales 
FROM KMS 
GROUP BY Product_Category 
ORDER BY TotalSales DESC;
```
- 🏆 **Technology** had the highest sales: `5,984,248.18`

---

**2. Top 3 and Bottom 3 regions in terms of sales**
```sql
-- Top 3
SELECT TOP 3 Region, SUM(Sales) AS TotalSales 
FROM KMS 
GROUP BY Region 
ORDER BY TotalSales DESC;

-- Bottom 3 using CTE
WITH RegionSales AS (
  SELECT Region, SUM(Sales) AS TotalSales 
  FROM KMS 
  GROUP BY Region
)
SELECT TOP 3 * FROM RegionSales ORDER BY TotalSales ASC;
```
- **Top 3 Regions:** West, Ontario, Prairie  
- **Bottom 3 Regions:** Based on output

---

**3. Total sales of appliances in Ontario**
```sql
SELECT SUM(Sales) AS TotalAppliancesSale 
FROM KMS 
WHERE Product_Sub_Category = 'Appliances' AND Province = 'Ontario';
```
- **Total Sales:** `202,346.84`

---

**4. Bottom 10 customers by sales**
```sql
SELECT TOP 10 Customer_Name, SUM(Sales) AS TotalSales 
FROM KMS 
GROUP BY Customer_Name 
ORDER BY TotalSales ASC;
```
- Sample names: Jeremy Farry, Natalie DeCherney, Nicole Fjeld, etc.

---

**5. Shipping method with highest cost**
```sql
SELECT TOP 1 Ship_Mode, SUM(Shipping_Cost) AS TotalShippingCost 
FROM KMS 
GROUP BY Ship_Mode;
```
- 💰 **Delivery Truck** had the highest shipping cost: `51,971.34`

---

### Case Scenario II

**6a. Top customers by sales**
```sql
SELECT TOP 5 Customer_Name, SUM(Sales) AS TotalSales 
FROM KMS 
GROUP BY Customer_Name 
ORDER BY TotalSales DESC;
```
- **Top Customers:** Emily Phan, Deborah Brumfield, Roy Skaria, etc.

**6b. What one of them buys**
```sql
SELECT Product_Category, COUNT(*) AS PurchaseCount 
FROM KMS 
WHERE Customer_Name IN ('Emily Phan', 'Deborah Brumfield', 'Roy Skaria', 'Sylvia Foulston', 'Grant Carroll') 
GROUP BY Product_Category;
```
- Categories: **Office Supplies**, **Technology**, **Furniture**

---

**7. Small business customer with highest sales**
```sql
SELECT TOP 1 Customer_Name, SUM(Sales) AS TotalSales 
FROM KMS 
WHERE Customer_Segment = 'Small Business' 
GROUP BY Customer_Name 
ORDER BY TotalSales DESC;
```
- 🏅 **Dennis Kane**: `753,675.31`

---

**8. Corporate customer with most orders**
```sql
SELECT TOP 1 Customer_Name, COUNT(Order_ID) AS OrderCount 
FROM KMS 
WHERE Customer_Segment = 'Corporate' AND YEAR(Order_Date) BETWEEN 2009 AND 2012 
GROUP BY Customer_Name 
ORDER BY OrderCount DESC;
```
- 🧾 **Adam Hart** placed `27` orders

---

**9. Most profitable consumer customer**
```sql
SELECT TOP 1 Customer_Name, SUM(Profit) AS TotalProfit 
FROM KMS 
WHERE Customer_Segment = 'Consumer' 
GROUP BY Customer_Name 
ORDER BY TotalProfit DESC;
```
- 💼 **Emily Phan**: `34,005.44` in total profit

---

**10. Customers who returned items + segment**
```sql
SELECT DISTINCT k.Customer_Name, k.Customer_Segment 
FROM KMS k 
JOIN Order_Status o ON k.Order_ID = o.Order_ID 
WHERE o.Status = 'Returned';
```

**'419'** customers returned their order

**Result Preview:**

| Customer Name       | Customer Segment |
|---------------------|------------------|
| Tracy Blumstein     | Consumer         |
| Allen Schofield     | Corporate        |
| Sydney Linden       | Corporate        |
| Mary Nachtrieb      | Home Office      |
| Catherine Abelman   | Home Office      |
| Gregory Shilling    | Consumer         |
| Kathleen Norgard    | Consumer         |
| Johnny Keffer       | Corporate        |
| Charles Cajigas     | Small Business   |
| Jesse Ingraham      | Home Office      |
```

---

**11. Was shipping mode used appropriately for priority?**
```sql
SELECT Order_Priority, Ship_Mode, COUNT(*) AS NumOrders, AVG(Shipping_Cost) AS AvgShippingCost 
FROM KMS 
GROUP BY Order_Priority, Ship_Mode 
ORDER BY Order_Priority, Ship_Mode;
```
- Answer:
  - **Critical** orders often used **Express Air** and **Delivery Truck**
  - **Low** priority sometimes used **Air**, which raises cost concerns
  - KMS could better align shipping methods with order urgency

---

## 💡 Observations and Insights

- **Technology** is the top-selling category  
- **West, Ontario, and Prairie** are the most profitable regions  
- A few **top customers** contribute significantly to revenue  
- **Express Air** is used frequently even for low-priority orders  
- **Return data** shows a small subset of customers returning goods  

---

## ✅ Recommendations

- Align shipping methods to match order priority and reduce unnecessary shipping costs  
- Offer loyalty incentives to high-performing customers to drive repeat purchases  
- Launch marketing campaigns in low-performing regions to boost awareness  
- Improve return process tracking and investigate frequent returners  
- Consider bundled product offerings to increase average order value  

---

## 🚫 Limitations

- Dataset only covers 2009–2012; trends may have changed  
- No direct delivery duration or customer feedback available  
- Return analysis is dependent on accuracy of `Order_Status` join  
