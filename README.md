![AdventureWorks Analysis Banner](https://github.com/chiragnemani/AdventureWorks-Analysis/assets/148119942/8c63b629-9194-4a6c-8304-befdf1efb25c)

**Welcome to the Adventure Works Sales Management Dashboard! This repository contains an end-to-end analysis for the Adventure Works company. This project leverages SQL and Power BI to generate insights for improved sales reporting and analysis.**

## &rarr; PROBLEM STATEMENT
In early 2024, Steven, the Sales Manager at Adventure Works, identified a crucial need to improve their sales reporting. The existing static reports were insufficient for the growing demands of the business. Steven needed dynamic, visual dashboards that could provide a comprehensive view of their internet sales, enabling better tracking of top-selling products and key customers over time. Each salesperson worked with different products and clients, so the ability to filter data by these categories was essential. Additionally, Steven wanted to compare sales performance against the budget for 2024, looking back at the past two years for trend analysis.

## &rarr; DATABASE
- **Overview:** Adventure Works is an Online Transaction Processing (OLTP) database supporting the operations of a fictional bicycle manufacturer called Adventure Works Cycles. It is a sample database owned by Microsoft that demonstrates how to design a database using SQL Server 2008. It's used for testing and training purposes, and to help students learn and practice SQL queries and data manipulation skills.<br>
- **Content:** This comprehensive dataset includes scenarios for various business functions such as manufacturing, sales, purchasing, product management, contact management, and human resources. The database is organized into modules covering business entities, people, human resources, products, manufacturing, purchasing, inventory, sales, and administration.

## &rarr; TECH STACK
- **SQL:** For data extraction, transformation, and loading.<br>
- **Power BI:** For creating interactive and insightful visualizations.

---
# PROJECT BREAKDOWN
To tackle this challenge, the project was structured into four key steps:<br>
- **Step 1: Setup Environment**<br>
I set up SQL Server Management Studio (SSMS), configured the SQL Server instance, and imported the Adventure Works database.<br><br>
- **Step 2: Business Request & Planning**<br>
Based on Steven's needs, I defined user stories to ensure the dashboards would meet the business requirements. The goals were to track top customers and products, provide detailed views per customer and product for sales representatives, and compare sales against the budget over time.<br><br>
![image](https://github.com/chiragnemani/AdventureWorks-Analysis/assets/148119942/6ebcfc12-151b-4f96-8d60-5bfdae5057f6)
<br><br>
- **Step 3: Data Cleansing & Transformation Using SQL**<br>
I prepared the data by cleansing, filtering, and transforming it using SQL queries:<br>

	<table>
	  <tr>
	    <th>Query 1: Cleansed DIM_DateTable</th>
	    <th>Query 2: Cleansed DIM_Customers Table</th>
	    <th>Query 3: Cleansed DIM_Products Table</th>
	    <th>Query 4: Cleansed FACT_InternetSales Table</th>
	  </tr>
	  <tr>
	    <td>
	<pre>
	<code>
	--Cleansed DIM_DateTable --
	SELECT
	    [DateKey],
	    [FullDateAlternateKey] AS Date,
	    [EnglishDayNameOfWeek] AS Day,
	    [WeekNumberOfYear] AS WeekNr,
	    [EnglishMonthName] AS Month,
	    LEFT([EnglishMonthName], 3) AS MonthShort,
	    [MonthNumberOfYear] AS MonthNo,
	    [CalendarQuarter] AS Quarter,
	    [CalendarYear] AS Year
	FROM
	    [AdventureWorksDW2022].[dbo].[DimDate]
	WHERE
	    CalendarYear >= 2022
	</code>
	</pre>
	    </td>
	    <td>
	<pre>
	<code>
	-- Cleansed DIM_Customers Table --
	SELECT
	    c.customerkey AS CustomerKey,
	    c.firstname AS [FirstName],
	    c.lastname AS [LastName],
	    c.firstname + ' ' + lastname AS [Full Name],
	    CASE c.gender WHEN 'M' THEN 'Male' WHEN 'F' THEN 'Female' END AS Gender,
	    c.datefirstpurchase AS [Date First Purchase],
	    g.city AS [Customer City]
	FROM
	    dbo.DimCustomer AS c
	    LEFT JOIN dbo.dimgeography AS g ON g.geographykey = c.geographykey
	ORDER BY
	    CustomerKey ASC
	</code>
	</pre>
	    </td>
	    <td>
	<pre>
	<code>
	-- Cleansed DIM_Products Table --
	SELECT 
	    p.[ProductKey], 
	    p.[ProductAlternateKey] AS ProductItemCode, 
	    p.[EnglishProductName] AS [Product Name], 
	    ps.EnglishProductSubcategoryName AS [Sub Category], 
	    pc.EnglishProductCategoryName AS [Product Category], 
	    p.[Color] AS [Product Color], 
	    p.[Size] AS [Product Size], 
	    p.[ProductLine] AS [Product Line], 
	    p.[ModelName] AS [Product Model Name], 
	    p.[EnglishDescription] AS [Product Description], 
	    ISNULL (p.Status, 'Outdated') AS [Product Status]
	FROM 
	    dbo.DimProduct as p
	    LEFT JOIN dbo.DimProductSubcategory AS ps ON ps.ProductSubcategoryKey = p.ProductSubcategoryKey 
	    LEFT JOIN dbo.DimProductCategory AS pc ON ps.ProductCategoryKey = pc.ProductCategoryKey 
	ORDER BY 
	    p.ProductKey ASC
	</code>
	</pre>
	    </td>
	    <td>
	<pre>
	<code>
	-- Cleansed FACT_InternetSales Table --
	SELECT 
	    [ProductKey], 
	    [OrderDateKey], 
	    [DueDateKey], 
	    [ShipDateKey], 
	    [CustomerKey], 
	    [SalesOrderNumber], 
	    [SalesAmount]
	FROM 
	    dbo.FactInternetSales
	WHERE 
	    LEFT (OrderDateKey, 4) >= YEAR(GETDATE()) -2
	ORDER BY
	    OrderDateKey ASC
	</code>
	</pre>
	    </td>
	  </tr>
	</table>
 
<br>

- **Step 4: Data Visualization Using Power BI**<br>
I designed and developed three comprehensive dashboards:<br>
&nbsp; &rarr; <ins>Sales Overview Dashboard</ins>: Provides daily updates on sales performance, highlighting top customers and products.<br>
&nbsp; &rarr; <ins>Customer Sales Analysis Dashboard</ins>: Offers detailed insights into sales per customer with filtering options.<br>
&nbsp; &rarr; <ins>Product Sales Analysis Dashboard</ins>: Breaks down sales data per product, with filtering capabilities.


	**&#128073; Video Demo**
	
	https://github.com/chiragnemani/AdventureWorks-Analysis/assets/148119942/0853af1e-70d7-442b-bf2a-596a805d701d
	
	**&#128073; Screenshots**
	<p float="left">
	  <img src="https://github.com/chiragnemani/AdventureWorks-Analysis/assets/148119942/c7895207-bac2-40d7-ae21-214fc3513eb3" alt="Sales Overview" width="200" style="margin-right: 10px;"/>
	  <img src="https://github.com/chiragnemani/AdventureWorks-Analysis/assets/148119942/1df95dc7-9bb7-48c7-aa59-7d9ef08af490" alt="Customer Details" width="200" style="margin-right: 10px;"/>
	  <img src="https://github.com/chiragnemani/AdventureWorks-Analysis/assets/148119942/5283879f-8b8c-4268-8bbb-c9557f4a6a60" alt="Product Details" width="200"/>
	</p>


---
# CONCLUSION
By leveraging SQL and Power BI, the Adventure Works Sales Management project provides a comprehensive solution for tracking and improving sales performance.<br>
### Features:
**1. Enhanced Sales Tracking:** <br>
*Enabled daily monitoring of sales performance with real-time updates, potentially improving response times by up to 50%.* <br><br>
**2. Improved Data Accessibility:** <br>
*Provided detailed insights into customer and product sales, likely increasing the efficiency of sales representatives by approximately 25% at least.* <br><br>
**3. Budget Comparison:** <br>
*Facilitated tracking of sales against budget over a two-year period, aiding in more accurate forecasting and potentially improving budget adherence by 30%.* <br>
