# E-Commerce Sales & Customer Analytics Dashboard

## 📊 Project Overview

The **E-Commerce Sales & Customer Analytics Dashboard** is an interactive Power BI project designed to analyze sales performance, profitability, product performance, customer behavior, and geographical sales distribution.

The project follows an end-to-end data analytics workflow:

**Raw Excel Data → Data Cleaning → Data Modeling → DAX Measures → Interactive Dashboard → Business Insights**

The dashboard helps business stakeholders understand revenue performance, identify high-performing products and customers, analyze profitability, and evaluate discount patterns.

---

## 🎯 Business Problem

An e-commerce business needs a clear view of its sales and customer performance to answer questions such as:

- How much revenue is being generated?
- How profitable is the business?
- Which products and categories generate the most sales?
- Which customers contribute the most revenue?
- Which states and cities perform best?
- How are sales changing month by month?
- How do discounts affect profitability?
- Are there any data-quality issues that could affect analysis?

This project addresses these questions through an interactive Power BI dashboard.

---

## 🎯 Project Objectives

The main objectives of this project were to:

- Analyze overall sales and profitability
- Track key business KPIs
- Identify top-performing products and categories
- Analyze customer purchasing behavior
- Understand geographical sales performance
- Analyze monthly sales trends
- Evaluate discount patterns and profitability
- Build an interactive and business-focused dashboard
- Demonstrate an end-to-end Power BI data analytics workflow

---

# 📁 Dataset

The project uses an Excel workbook containing five related tables.

### 1. Customers

Contains customer information.

**Key columns:**
- CustomerID
- CustomerName
- Gender
- City
- State

### 2. Products

Contains product information.

**Key columns:**
- ProductID
- ProductName
- Category
- UnitPrice
- CostPrice

### 3. Orders

Contains order-level information.

**Key columns:**
- OrderID
- CustomerID
- OrderDate
- SalesChannel
- Status

### 4. Order_Details

Contains individual order-line information.

**Key columns:**
- OrderID
- ProductID
- Quantity
- UnitPrice
- Discount

### 5. Date

A dedicated date table used for time-based analysis.

**Key columns:**
- Date
- Year
- MonthNo
- Month
- Quarter

---

# 🧹 Data Cleaning & Transformation

Data preparation was performed using **Power Query**.

The following transformations were carried out:

### Customer Data
- Removed duplicate customer records
- Standardized data types
- Verified customer identifiers

### Product Data
- Standardized category values such as `Stationary` / `stationery` to `Stationery`
- Corrected the missing UnitPrice for USB Hub using supporting transaction data
- Created a `CostPrice` column using:

```text
CostPrice = UnitPrice × 70%
### Order Data
- Converted OrderDate from text into a proper Date data type using the appropriate locale
- Standardized `online` to `Online`
- Retained a missing Order Status where the correct value could not be determined from the available data

### Order Details
- Standardized numeric data types
- Replaced a missing Discount with `0` where appropriate
- Retained a missing Quantity value rather than inventing a quantity

### Data Quality Validation

After the cleaning and transformation process, the tables were checked for Power Query errors.

**Result: 0 errors**

---

# 🔗 Data Model

A relational data model was created using a **star-like structure with Orders and Order_Details at the transactional level and dimension tables supporting analysis.**

### Relationships

```text
Customers
    1
    |
    *
  Orders
    1
    |
    *
Order_Details
    *
    |
    1
 Products

Date
    1
    |
    *
  Orders
Relationships implemented
*Customers[CustomerID] → Orders[CustomerID]
*Orders[OrderID] → Order_Details[OrderID]
*Products[ProductID] → Order_Details[ProductID]
*Date[Date] → Orders[OrderDate]

Relationships were configured as one-to-many with appropriate single-direction filtering.
🧮 DAX Measures

Several DAX measures were created to calculate the major business KPIs.

Total Sales
Total Sales =
SUMX(
    Order_Details,
    Order_Details[Quantity] *
    Order_Details[UnitPrice] *
    (1 - Order_Details[Discount])
)
Total Orders

Total Orders =
DISTINCTCOUNT(Orders[OrderID])

Total Customers
Total Customers =
DISTINCTCOUNT(Customers[CustomerID])

Total Quantity
Total Quantity =
SUM(Order_Details[Quantity])

Average Order Value
Average Order Value =
DIVIDE(
    [Total Sales],
    [Total Orders]
)
Total Profit
Total Profit =
SUMX(
    Order_Details,
    Order_Details[Quantity] *
    (
        Order_Details[UnitPrice] * (1 - Order_Details[Discount])
        - RELATED(Products[CostPrice])
    )
)
Profit Margin %
Profit Margin % =
DIVIDE(
    [Total Profit],
    [Total Sales],
    0
)
Average Discount %
Average Discount % =
AVERAGE(Order_Details[Discount])

📊 Dashboard Pages
1️⃣ Executive Sales Overview

This page provides a high-level view of overall business performance.

KPIs
Total Sales — ₹341.07K
Total Profit — ₹89.53K
Total Orders — 20
Total Customers — 12
Total Quantity — 49
Average Order Value — ₹17.05K
Profit Margin — 26.25%
Visualizations
Monthly Sales Trend
Sales by Category
Sales by State
2️⃣ Product & Profitability Analysis

This page focuses on product performance and profitability.

Visualizations
Top Products by Sales
Category Sales vs Profit
Discount vs Profitability
3️⃣ Customer Analysis

This page focuses on customer-level and geographical analysis.

Visualizations
Top Customers by Sales
Customer Performance Matrix
Sales by City
Interactive Filters
City
Gender
Sales Channel
💡 Key Business Insights
1. Overall Performance

The business generated approximately ₹341.07K in sales and ₹89.53K in profit, resulting in a 26.25% profit margin.

The business is profitable, while increasing order volume represents an opportunity for further growth.

2. Monthly Sales Trend

January recorded the strongest sales performance.

Sales declined significantly in February, showed partial recovery in March, and declined again in April.

This indicates noticeable monthly sales fluctuations and suggests that factors such as product demand, promotions, customer activity, or seasonality should be investigated further.

3. Category Performance

Electronics is the strongest revenue-generating category, followed by Furniture.

Accessories and Stationery contribute comparatively lower sales.

4. Product Performance

The Laptop is the leading product by sales.

Its strong performance makes inventory availability and complementary-product cross-selling important opportunities.

5. Geographical Performance

Tamil Nadu is the strongest-performing state in terms of sales.

Among cities, Chennai is a major contributor.

6. Customer Performance

A small group of customers contributes a significant portion of overall sales.

High-value customers such as Anita Sharma, Arjun Rao, Meena Iyer, and Rohan Das represent important customer segments for retention and personalized offers.

7. Discount & Profitability

The analysis shows that higher discounts do not automatically result in higher profitability.

Discount strategies should therefore be evaluated at the product level to balance sales growth with profit protection.

8. Data Quality

The project identified several data-quality issues, including:

Duplicate customer records
Inconsistent category spelling
Inconsistent sales-channel capitalization
Missing product pricing
Missing transaction quantity
Missing discount values
📌 Business Recommendations
Maintain strong inventory availability for top-selling products, particularly laptops and other electronics.
Focus on high-value customers through loyalty programs and personalized offers.
Investigate monthly sales fluctuations to understand the reasons behind significant changes in performance.
Evaluate discount strategies carefully to protect profitability.
Explore growth opportunities in lower-performing categories such as Accessories and Stationery.
Strengthen regional marketing strategies based on state and city-level performance.
Improve data-entry validation to reduce duplicate, inconsistent, and missing data.
🛠️ Tools & Technologies
Microsoft Excel — Data source
Power Query — Data cleaning and transformation
Power BI — Data visualization and dashboard development
DAX — KPI and business metric calculations
📚 Key Power BI Concepts Demonstrated
Data Import
Power Query
Data Cleaning
Data Transformation
Data Types
Duplicate Removal
Missing Value Handling
Data Modeling
Relationships
One-to-Many Cardinality
Star Schema Concepts
DAX Measures
SUMX
SUM
DISTINCTCOUNT
DIVIDE
AVERAGE
RELATED
KPI Cards
Line Charts
Bar Charts
Column Charts
Combo Charts
Scatter Charts
Donut Charts
Matrix Visuals
Slicers
Page Navigation
Interactive Dashboard Design
📂 Project Structure
E-Commerce Power BI Project
│
├── Dataset
│   └── Ecommerce_PowerBI_Practice_Dataset.xlsx
│
├── Power BI File
│   └── Ecommerce_Sales_Customer_Analytics.pbix
│
├── Screenshots
│   ├── Executive_Sales_Overview.png
│   ├── Product_Profitability_Analysis.png
│   └── Customer_Analysis.png
│
└── Project Documentation
    └── Project_Documentation.pdf
🎯 Project Outcome

This project demonstrates an end-to-end approach to solving a business analytics problem using Power BI.

From cleaning raw Excel data to building relationships, creating DAX measures, designing interactive dashboards, and generating business insights, the project focuses on transforming raw data into meaningful information for decision-making.

👩‍💻 Author

Reshma Krishnapillai

Aspiring Data Analyst | Excel | SQL | Power BI | Python
