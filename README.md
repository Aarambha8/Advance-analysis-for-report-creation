Advanced SQL Analysis and Report Views (SQL Server)

Advanced SQL on a star-schema sales warehouse: change over time, running totals, year-over-year performance, segmentation and part-to-whole shares. Two reporting views, one for customers and one for products, sit on top.

Credit: The structure and dataset follow the free SQL data analytics course by Data With Baraa.

Dataset

Sample sales data from Dec 2010 to Jan 2014 in three tables:

Table	Rows	Holds
gold.fact_sales	60,398	Order lines (27,659 orders)
gold.dim_customers	18,484	Customer attributes
gold.dim_products	295	Product attributes (130 appear in sales)

The CSV files are in Datasets/.

Key findings

Figures come from the CSV files in Datasets/. 2010 and 2014 are partial years, so yearly comparisons use 2011 to 2013.

2013 vs 2012: orders rose 551% and sales rose 180%, but revenue per order fell 57%.
Category mix: bikes were 100% of sales until 2012. Accessories (4.1%) and clothing (2.0%) first appeared in 2013. Across all years, bikes are 96.5% of sales.
Customers: the top 10% of customers bring in 40% of sales. VIP customers are 9% of customers and 37% of sales. 63% of customers ordered once.
Products: 66 of the 130 products sold are High-Performers and generate 94% of product sales.
Scripts
Script	Question it answers	SQL used
01_change_over_time.sql	How do sales, customers and quantity change by year and month?	DATETRUNC, date functions, aggregates
02_cumulative_analysis.sql	What are the running total and moving average of monthly sales?	SUM() OVER, AVG() OVER
03_performance_analysis.sql	How does each product's yearly sales compare with its average and with the previous year?	LAG(), AVG() OVER, CASE, CTE
04_data_segmentation.sql	How do products fall into cost ranges? How do customers group by spend?	CASE, CTEs, GROUP BY
05_part_to_whole_analysis.sql	Which categories contribute most to sales?	SUM() OVER, CTE
12_report_customers.sql	One customer view with segments and KPIs	View, CTEs
13_report_products.sql	One product view with segments and KPIs	View, CTEs
Segment rules in the report views
Customers: VIP is 12 or more months of history and over 5,000 spent. Regular is 12 or more months and 5,000 or less. New is under 12 months.
Products: High-Performer is over 50,000 in sales. Mid-Range is 10,000 to 50,000. Low-Performer is under 10,000.
Repository structure
Datasets/        CSV files (fact, dimensions, two report extracts)
Scripts/         00_database_creation.sql
                 01_change_over_time.sql
                 02_cumulative_analysis.sql
                 03_performance_analysis.sql
                 04_data_segmentation.sql
                 05_part_to_whole_analysis.sql
Report_scripts/  12_report_customers.sql
                 13_report_products.sql
How to run
Install SQL Server (the Express edition works) and SQL Server Management Studio.
Clone this repository.
Open Scripts/00_database_creation.sql and change the file paths in the BULK INSERT statements so they point to your local Datasets/ folder. Run the script. It drops and recreates the DataWarehouseAnalytics database, so back up anything you need first.
Run scripts 01 to 05 in any order.
Run 12_report_customers.sql and 13_report_products.sql to create the two views.
Notes
The report views compute recency with GETDATE(). The data ends in 2014, so recency values are large and grow over time.
License

MIT. See LICENSE.
