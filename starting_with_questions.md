Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
select country, city, Sum(transactionrevenue) as HighestRevenue 
from allsessions 
where transactionrevenue <> 0
group by city, country
order by HighestRevenue desc
limit 5;

Answer:
"country"	"city"	"highestrevenue"
"United States"	"N/A"	2190950000
"United States"	"Sunnyvale"	200000000

**Question 2: What is the average number of products ordered from visitors in each city and country?**
SQL Queries:
select country, city, round(avg(orderedquantity),3) as AvgProductOrdered from products p
join allsessions a
on p.sku = a.productsku
group by city, country
order by AvgProductOrdered desc;


Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
SELECT a.country as country, a.city as city, a.v2ProductCategory as productcategory, SUM(an.units_sold) 
as totalUnitsSold FROM allsessions AS a JOIN analytics AS an ON a.visitId = an.visitId JOIN products AS p ON a.productSKU = p.SKU WHERE a.country != '(not set)' AND a.city != '(not set)' AND a.v2ProductCategory != '(not set)' GROUP BY a.country, a.city, a.v2ProductCategory 
HAVING SUM(an.units_sold) IS NOT NULL ORDER BY a.country, a.city, totalUnitsSold DESC


Answer: Yes there is a pattern there was lots of Google products, pet products, men's products and electronics.


**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
WITH SalesData AS ( SELECT Customers.City, Customers.Country, Products.ProductName, order_details.UnitPrice * order_details.Quantity AS SalesAmount FROM Customers INNER JOIN Orders ON Customers.CustomerID = Orders.CustomerID INNER JOIN order_details ON Orders.OrderID = order_details.OrderID INNER JOIN Products ON order_details.ProductID = Products.ProductID ), AggregatedSales as ( SELECT City, Country, ProductName, SUM(SalesAmount) AS TotalSales FROM SalesData GROUP BY City, Country, ProductName )

WITH ranked_products as ( SELECT als.country, p.name, RANK() OVER (PARTITION BY als.country ORDER BY ana.units_sold DESC) as rank FROM all_sessions as als JOIN analytics as ana ON als.visitid = ana.visitid JOIN products as p ON als.productsku = p.sku GROUP BY als.country, p.name, ana.units_sold ORDER BY UNITS_SOLD )

SELECT country, rank FROM ranked_products GROUP BY country, Rank

Answer: Home/Apparel/Men's/Men's T-Shirt (SPF-15 Slim & Slender Lip Balm, Alpine Style Backpack, Twill Cap). There is a pattern of home security products.

**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries: SELECT city, country, SUM(totaltotalTransactionRevenue)AS totalRevenue, COUNT(DISTINCT "transactionId")AS totalTransactions FROM all_sessions GROUP BY city, country;

SELECT country, city, SUM(totalTransactionRevenue) AS total_revenue FROM all_sessions WHERE country in ('United States', 'Israel', 'Australia') GROUP BY country, city HAVING SUM(totalTransactionRevenue) >= 0 ORDER BY total_revenue DESC

Answer: USA - San Francisco has the highest revenue.






