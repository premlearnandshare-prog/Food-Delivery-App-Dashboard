# Food Delivery Analytics Dashboard | Power BI

An interactive, multi-page Power BI dashboard that analyzes food delivery app data across **sales, customers, restaurants and delivery operations**. The project covers the full BI workflow: data cleaning in Power Query, data modeling, DAX measures, conditional columns and dashboard design.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Business Objectives](#business-objectives)
3. [Dataset](#dataset)
4. [Tools and Technologies](#tools-and-technologies)
5. [Project Workflow](#project-workflow)
6. [Data Model](#data-model)
7. [DAX Measures](#dax-measures)
8. [Dashboard Pages](#dashboard-pages)
9. [Key Features](#key-features)
10. [Business Questions Answered](#business-questions-answered)
11. [Repository Structure](#repository-structure)
12. [How to Use](#how-to-use)
13. [Author](#author)

---

## Project Overview

Food delivery platforms generate large volumes of data every day: orders, customers, restaurants and delivery times. This project turns that raw data into a clear, decision-ready dashboard that helps stakeholders monitor revenue, understand customer behavior, evaluate restaurant performance and improve delivery efficiency.

## Business Objectives

- Track overall business performance (revenue, orders, average order value).
- Identify top-performing cities, cuisines and restaurants.
- Understand customer segments and measure repeat customer behavior.
- Monitor delivery performance, late deliveries and cancellations.
- Provide drill-through access to detailed restaurant-level insights.

## Dataset

The data is stored in an Excel workbook (`dataset.xlsx`) with four tables:

| Table | Rows | Description |
|---|---|---|
| **Orders** | 200,000 | Order transactions: `OrderID`, `RestaurantID`, `CustomerID`, `OrderDate`, `OrderValue`, `DeliveryTimeMins`, `OrderStatus`, `Quantity` |
| **Customers** | 50,000 | `CustomerID`, `CustomerName`, `City`, `SignupDate`, `CustomerType` |
| **Restaurants** | 50,000 | `RestaurantID`, `name`, `address`, `location`, `rest_type`, `cuisines`, `dish_liked`, `rate`, `votes`, `online_order`, `book_table`, `approx_cost(for two people)`, `listed_in(type)`, `listed_in(city)` |
| **DateTable** | 50,000 | Calendar table: `Date`, `Year`, `MonthNo`, `MonthName`, `Quarter`, `Day` |

## Tools and Technologies

- **Power BI Desktop**: data modeling, visualization, report design
- **Power Query Editor (M)**: data cleaning and transformation
- **DAX**: measures and calculated logic
- **Microsoft Excel**: source data

## Project Workflow

### 1. Data Loading
The Excel workbook was imported into Power BI Desktop and all four tables were loaded into Power Query Editor.

### 2. Data Cleaning and Transformation (Power Query Editor)
Each table was reviewed and prepared before loading into the model. Typical cleaning steps applied:

- Promoted headers and set the correct data types (numbers, dates, text)
- Removed duplicates, blanks and error values
- Standardized text columns and fixed inconsistent values
- Renamed columns and tables for clarity

After the transformations were complete, **Close & Apply** was used to load the clean data into the model.

### 3. Data Modeling
Relationships were created between the tables to build a **star schema**:

- `Orders` is the central **fact table** and holds the foreign keys.
- `Restaurants`, `Customers` and `DateTable` are **dimension tables**, each joined through its primary key.
- Relationships are one-to-many, with the dimension on the "one" side and `Orders` on the "many" side.

### 4. Measures and Conditional Columns
- All DAX **measures** were created in the `Orders` table to keep the model organized.
- **Conditional columns** were added to the dimension tables to create business-friendly categories, such as customer type groups, restaurant cost categories (Budget / Premium) and delivery status.

### 5. Dashboard Design
A clean, consistent yellow and black theme was used across six report pages, with page navigation, slicers, drill-through and interactive charts.

## Data Model

```
        Customers                Restaurants               DateTable
        (CustomerID - PK)        (RestaurantID - PK)       (Date - PK)
              \                        |                       /
               \                       |                      /
                +--------------->  Orders  <----------------+
                                (Fact table)
                         CustomerID, RestaurantID, OrderDate (FK)
```

An additional disconnected **KPI** table powers a dynamic KPI selector, so users can switch the metric shown on the trend chart.

## DAX Measures

Measures are created in the `Orders` table. Main groups:

| Category | Measures |
|---|---|
| **Sales** | Total Sales, Average Sales, Average Orders Value, Revenue per Restaurant |
| **Orders** | Total Orders, Delivered Orders, Cancelled Orders |
| **Delivery** | Avg Delivery Time, Late Orders, Late Delivery % |
| **Customers** | Total Customers, Repeat Customers, Repeat Customer %, Order per Customer |
| **Restaurants** | Total Restaurant, Average Rating, Average Votes |
| **Dynamic KPI** | Select KPI Value (works with the KPI slicer) |

Example: repeat customer logic

```DAX
Repeat Customers =
SUMX(
    VALUES(Orders[CustomerID]),
    IF(CALCULATE(DISTINCTCOUNT(Orders[OrderID])) > 1, 1, 0)
)

Repeat Customer % =
DIVIDE([Repeat Customers], DISTINCTCOUNT(Orders[CustomerID]))
```

## Dashboard Pages

| # | Page | Purpose |
|---|---|---|
| 1 | **Executive Overview** | High-level KPIs (Total Revenue, Total Orders, Average Order Value, Average Delivery Time), revenue and order trends, revenue by city, order status split and a top restaurants table |
| 2 | **Customer Analysis** | Total customers, repeat customers and repeat %, orders per customer, orders by customer type, sign-up trend and customer-level table |
| 3 | **Restaurant Performances** | Total restaurants, average rating and votes, revenue per restaurant, rating by cuisine, budget vs premium split and rating vs orders scatter plot |
| 4 | **Delivery and Operations** | Average delivery time, late orders and late delivery %, cancelled orders, city-wise delivery time and cancellations, delivery trend |
| 5 | **Advanced Insights** | Dynamic KPI view: choose a KPI from the slicer and see its trend over time |
| 6 | **Restaurant Details** | Drill-through page with restaurant-level KPIs, order trend, revenue trend and customer mix for any selected restaurant |

## Key Features

- **Multi-page navigation** with a page navigator on every page
- **Drill-through** from restaurant lists to a detailed restaurant page, with a Back button
- **Dynamic KPI selector** built with a disconnected table and a slicer
- **Conditional columns** for clean business segmentation
- **Star schema model** with all measures centralized in the `Orders` table
- **Consistent design** with a custom color theme, readable card visuals and clear titles

## Business Questions Answered

- How much revenue and how many orders does the platform generate, and how are they trending over time?
- Which cities contribute the most revenue?
- What share of orders are delivered, pending or cancelled?
- How many customers are repeat customers, and how often do they order?
- Which cuisines and restaurants have the best ratings and sales?
- How do budget and premium restaurants compare?
- Which cities have the longest delivery times and the most cancellations?
- How does any individual restaurant perform over time?

## Repository Structure

```
Food-Delivery-Dashboard/
|-- dataset.xlsx                                   # Source data (Orders, Customers, Restaurants, DateTable)
|-- Power_bi_Food_Delivery_Dashboard_Project.pbix  # Power BI report file
|-- screenshots/                                   # Dashboard page screenshots
|-- README.md
```

## How to Use

1. Download `Power_bi_Food_Delivery_Dashboard_Project.pbix` and `dataset.xlsx`.
2. Open the `.pbix` file in **Power BI Desktop** (latest version recommended).
3. If prompted, update the data source path: **Home > Transform data > Data source settings** and point it to your local `dataset.xlsx`.
4. Click **Refresh** and explore the pages. Use **Ctrl + Click** on navigation buttons inside Power BI Desktop.

## Author

**Prem Kumar**
BCA Student | Aspiring Data Analyst
Skills: Excel, Power BI, SQL
Email: prem.learnandshare@gmail.com

---

If you found this project useful, feel free to star the repository.
