# Kultra-Mega-Stores-Inventory-
KMS Sales Performance Analysis: A SQL-centric project focused on dimensional modeling and data analysis using historical sales data to uncover business insights.   Sources and related content

# KMS Sales Performance Analysis 

## 1. Project Overview & Business Problem

**Kultra Mega Stores (KMS)**, a leading retailer of office supplies and furniture in Lagos, is looking to enhance its business intelligence capabilities, particularly for its Abuja division. As a newly engaged Business Intelligence Analyst, my task was to analyze historical order data from **2009 to 2012** (provided in an Excel file) to uncover key sales insights and findings.

This report addresses a series of critical business questions posed by the Business Manager, aiming to provide actionable intelligence for strategic decision-making, revenue optimization, and improved operational efficiency within the Abuja division.

**Key Objectives:**
* Identify top-performing and underperforming product categories and regions.
* Analyze customer behavior across different segments (Consumers, Small Businesses, Corporate).
* Evaluate shipping cost effectiveness against order priority.
* Provide data-driven recommendations to increase revenue and profitability.

## 2. Technical Approach & Tools

The project followed a standard Business Intelligence workflow:

1.  **Data Ingestion & Preparation:** The raw Excel order data was imported into a SQL Server database. Initial data cleaning and transformation were performed using SQL to ensure data quality and readiness for modeling.
2.  **SQL Querying & View Creation:** Advanced SQL skills were applied to write complex queries and create SQL Views. These views pre-calculated key metrics and aggregated data to answer specific business questions efficiently.


**Tools Used:**
* **Database Management System:** Microsoft SQL Server
* **Database Interaction & ETL:** SQL Server Management Studio (SSMS), 
* **Documentation & Version Control:** GitHub (`README.md`)

 Key Insights & Findings (Addressing Case Scenarios)
This section presents the answers to the business questions, supported by data and visualizations.

**Case Scenario I Analysis**
```
1. ***Product category with the highest sales?***
   
Select Product_Category, sum(sales) as Total_sales
from [dbo].[KMS Sql]
Group by Product_Category
Order by Total_sales desc;

2. ***The Top 3 and Bottom 3 regions in terms of sales?***
   
   ```Select top 3 region, sum(sales) as Total_sales
from [dbo].[KMS Sql]
Group by region
order by Total_sales desc;

```Select top 3 region, sum(sales) as Total_sales
from [dbo].[KMS Sql]
Group by region
order by Total_sales asc;

3. ***Total sales of appliances in Ontario? ***
```SELECT SUM(SALES) AS Total_sales
from [dbo].[KMS Sql]
where product_sub_category= 'appliances'
and PROVINCE ='Ontario';

4. *** The bottom 10 customers ***
```With Bottom10customers AS (
SELECT top 10 Customer_name, sum(sales) as total_revenue
from [dbo].[KMS Sql]
Group by customer_name
order by total_revenue asc
)
SELECT KS.ORDER_ID, ks.Order_date, ks.order_priority, ks.order_quantity, ks.sales, ks.discount, ks.ship_mode, ks.profit, ks.unit_price, 
ks.shipping_cost, ks.customer_name, ks.province, ks.region, ks.customer_segment, ks.product_category, ks.product_sub_category, ks.product_name,
ks.product_container, ks.product_base_margin, ks.ship_date, b.total_revenue
from [dbo].[KMS Sql] ks
inner join bottom10customers b ON KS.customer_name = b.customer_name

5. *** Theshipping method that incurred the most shipping cost?
Select top 1 ship_mode, sum(shipping_cost) As totalshippingcost
from [dbo].[KMS Sql]
Group by ship_mode
order by totalshippingcost desc;

6.*** The most valuable customers, and what products or services do they typically purchase?***

```WITH TopCustomers AS (
    SELECT TOP 10 Customer_name,
        SUM(Sales) AS Total_Revenue_Overall 
    FROM [dbo].[KMS Sql]
    GROUP BY Customer_name
    ORDER BY Total_Revenue_Overall DESC
)
SELECT
    ks.Customer_name,ks.Product_name,
    SUM(ks.Sales) AS Product_Specific_Revenue, 
    COUNT(*) AS Purchase_Count_For_Product 
FROM
    [dbo].[KMS Sql] ks
JOIN
    TopCustomers tc ON ks.Customer_name = tc.Customer_name 
GROUP BY
    ks.Customer_name,ks.Product_name
ORDER BY
    ks.Customer_name,
    Product_Specific_Revenue DESC;

7.*** Small business customer had the highest sales?***

```select top 1 customer_name, sum(sales) as Totalsales
from [dbo].[KMS Sql]
where Customer_Segment = 'Small Business'
group by Customer_Name
order by Totalsales desc;

8.***Corporate Customer that placed the most number of orders in 2009 – 2012?***

```select top 1 customer_name, Count(Order_Quantity) as Totalorder
from [dbo].[KMS Sql]
where Customer_Segment = 'Corporate'
and Order_Date between '01/01/2009' and '31/12/2012'
group by Customer_Name
order by Totalorder desc;

9. ***The most profitable consumer customer***

```select top 1 customer_name, sum(Profit) as Totalprofit
from [dbo].[KMS Sql]
where Customer_Segment = 'Consumer'
group by Customer_Name
order by Totalprofit desc;

10.***Customer that returned items, and what segment they belong to?
```SELECT
    ks.customer_name,
    ks.customer_segment, 
    os.status   
FROM
    [dbo].[KMS Sql] ks  
INNER JOIN
    [order_status] os ON ks.order_id = os.order_id;

11.***Utilization of shipping mode***
```select order_priority,ship_mode, Count(*) As NumOrders,
sum(shipping_cost) as Total_shipping_cost
from [dbo].[KMS Sql]
Group by order_priority,ship_mode
order by  Total_shipping_cost desc;


select order_priority,ship_mode, Count(*) As NumOrders,
sum(shipping_cost) as Total_shipping_cost
from [dbo].[KMS Sql]
where ship_mode In ('delivery truck', 'express air')
Group by order_priority,ship_mode
order by  Total_shipping_cost desc;```


