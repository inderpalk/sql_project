Question 1: Calculate the average order value for each visitor:

SQL Queries:

SELECT Vistors.VisitorID, AVG(order_details.UnitPrice * order_details.Quantity) AS AvgOrderValue FROM Visitors INNER JOIN Orders ON Visitors.VisitorID = Orders.VisitID INNER JOIN order_details ON Orders.OrderID = order_details.OrderID GROUP BY Visitor.VisitID;

Answer: 1

Question 2: What is thes total number of sales for each visitor

SQL Queries:

SELECT Visitor.VisitID, SUM(order_details.UnitPrice * order_details.Quantity) AS TotalSales FROM Visitors INNER JOIN Orders ON Visitors.CustomerID = Orders.VisitorID INNER JOIN order_details ON Orders.OrderID = order_details.OrderID GROUP BY Visitors.VisitID;

Answer: 1

Question 3: What is the total amount of revenue for each category of a product.

SQL Queries:

SELECT Categories.CategoryName, SUM(order_details.UnitPrice * order_details.Quantity) AS TotalRevenue FROM Categories INNER JOIN Products ON Categories.CategoryID = Products.CategoryID INNER JOIN order_details ON Products.ProductID = order_details.ProductID GROUP BY Categories.CategoryName;

Answer: 10+