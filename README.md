# SQL_classicmodels
Analyzing the database of a retailer specializing in scale models of classic cars.

# <h4>Database Schema</h4>

![MySQL-Sample-Database-Schema](https://github.com/HikariJadeEmpire/SQL_classicmodels/assets/118663358/d179ce3f-0434-4cd3-8f50-2f3e14dff0a7)

link : [:bookmark: classicmodels](https://www.mysqltutorial.org/mysql-sample-database.aspx)

# <h3>Q1 : Which country consistently achieves the highest average monthly revenue? Sort the countries for each month accordingly.</h3>

```ruby

USE classicmodels ;

SELECT
    t.country,
    ROUND(MAX(CASE WHEN t.months = 1 THEN t.total ELSE 0 END ),2) AS Jan ,
    ROUND(MAX(CASE WHEN t.months = 2 THEN t.total ELSE 0 END ),2) AS Feb ,
    ROUND(MAX(CASE WHEN t.months = 3 THEN t.total ELSE 0 END ),2) AS Mar ,
    ROUND(MAX(CASE WHEN t.months = 4 THEN t.total ELSE 0 END ),2) AS Apr ,
    ROUND(MAX(CASE WHEN t.months = 5 THEN t.total ELSE 0 END ),2) AS May ,
    ROUND(MAX(CASE WHEN t.months = 6 THEN t.total ELSE 0 END ),2) AS Jun ,
    ROUND(MAX(CASE WHEN t.months = 7 THEN t.total ELSE 0 END ),2) AS Jul ,
    ROUND(MAX(CASE WHEN t.months = 8 THEN t.total ELSE 0 END ),2) AS Aug ,
    ROUND(MAX(CASE WHEN t.months = 9 THEN t.total ELSE 0 END ),2) AS Sep ,
    ROUND(MAX(CASE WHEN t.months = 10 THEN t.total ELSE 0 END ),2) AS Oct ,
    ROUND(MAX(CASE WHEN t.months = 11 THEN t.total ELSE 0 END ),2) AS Nov ,
    ROUND(MAX(CASE WHEN t.months = 12 THEN t.total ELSE 0 END ),2) AS Decc ,
    ROUND(AVG(total),2) as total
FROM
	(SELECT MONTH(a.orderDate) AS months , a.country , SUM(b.quantityOrdered*b.priceEach) AS total
	FROM
		(SELECT a.orderDate , a.orderNumber, c.country
		FROM orders AS a
		LEFT JOIN customers AS c ON a.customerNumber = c.customerNumber
		) AS a
	LEFT JOIN orderdetails AS b
	ON a.orderNumber = b.orderNumber
	GROUP BY months, a.country ) 
AS t
GROUP BY t.country
order by total DESC ;

```

**Answer**

![sql01](https://github.com/HikariJadeEmpire/SQL_classicmodels/assets/118663358/e61c3659-d215-44e5-9fd6-a6733358c932)

# <h3>Q2 : 
Which product performs the best each month?</h3>

```ruby

```

**Answer**

XX

# <h3>Q3 : </h3>

```ruby

```

**Answer**

XX

