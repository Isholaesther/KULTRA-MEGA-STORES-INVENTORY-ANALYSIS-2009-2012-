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
- Data spans orders placed between 2009 and 2012

---

## 🛠️ Tools and Technique
- **SQL Server 2022** for querying and analysis
- Basic Excel for viewing and interpreting data summaries
- Relational joins and aggregate functions (`SUM`, `COUNT`, `AVG`)
- CTEs (Common Table Expressions) for advanced filtering and ranking

---

## 🔍 Exploratory Data Analysis

### Case Scenario I

**1. Product category with highest sales**
```sql
SELECT TOP 1 Product_Category, SUM(Sales) AS TotalSales 
FROM KMS 
GROUP BY Product_Category 
ORDER BY TotalSales DESC;
