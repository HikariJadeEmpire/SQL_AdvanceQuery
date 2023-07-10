# Advanced SQL query.
Analyzing the database of a retailer specializing in scale models of classic cars.

# <h4>Database Schema</h4>

![MySQL-Sample-Database-Schema](https://github.com/HikariJadeEmpire/SQL_classicmodels/assets/118663358/d179ce3f-0434-4cd3-8f50-2f3e14dff0a7)

<br>

link : [:bookmark: classicmodels](https://www.mysqltutorial.org/mysql-sample-database.aspx)

<br>

**Questions :**
- [Q1 : Which are the top 5 countries with the highest average monthly revenue production? Please sort the countries accordingly for each month.](https://github.com/HikariJadeEmpire/SQL_AdvanceQuery#q1--which-are-the-top-5-countries-with-the-highest-average-monthly-revenue-production-please-sort-the-countries-accordingly-for-each-month)
- [Q2 : Which product performs the best each month, disregarding the year?](https://github.com/HikariJadeEmpire/SQL_AdvanceQuery#q2--which-product-performs-the-best-each-month-disregarding-the-year)
- [Q3 : Plot the total revenue for each month and year.](https://github.com/HikariJadeEmpire/SQL_AdvanceQuery#q3--plot-the-total-revenue-for-each-month-and-year)

<br>

# <h3>Q1 : Which are the top 5 countries with the highest average monthly revenue production? Please sort the countries accordingly for each month.</h3>
<br>

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
ORDER BY total DESC
LIMIT 5
;

```
<br>

**Answer (Query result)**

| country | Jan | Feb | Mar | Apr | May | Jun | Jul | Aug | Sep | Oct | Nov | Decc | total |
|---------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|------|-------|
| USA | 210757.96 | 217707.85 | 238210.3 | 185634.17 | 284867.21 | 159182.38 | 188268.51 | 362668.24 | 129973.45 | 361965.15 | 667780.22 | 266264.61 | 272773.34 |
| Spain | 122199.36 | 120166.58 | 80394.19 | 29257.31 | 197333.34 | 83316.39 | 0.0 | 20009.53 | 44939.85 | 44214.56 | 188227.89 | 169330.09 | 99944.46 |
| France | 100614.44 | 74280.98 | 117995.88 | 116801.0 | 148800.97 | 4632.31 | 99496.62 | 1960.8 | 6066.78 | 72618.81 | 205193.19 | 58912.24 | 83947.84 |
| New Zealand | 0.0 | 0.0 | 65263.69 | 108122.9 | 23627.44 | 77930.74 | 67112.01 | 0.0 | 0.0 | 0.0 | 22963.6 | 111826.63 | 68121.0 |
| Australia | 27083.78 | 66327.05 | 29848.52 | 45864.03 | 60761.85 | 0.0 | 69235.38 | 0.0 | 51376.05 | 0.0 | 180250.57 | 31835.36 | 62509.18 |


# <h3>Q2 : Which product performs the best each month, disregarding the year?</h3>
<br>

```ruby

USE classicmodels ;

WITH table1 AS
	(SELECT MONTH(a.orderDate) AS months, b.productCode, b.quantityOrdered
	FROM orders AS a
	INNER JOIN 
		(SELECT a.orderNumber, a.productCode, a.quantityOrdered
		FROM orderdetails AS a
		WHERE NOT EXISTS 
			(SELECT * 
			FROM orderdetails AS b 
			WHERE b.orderNumber = a.orderNumber
			AND b.quantityOrdered > a.quantityOrdered )
		) AS b 
	ON b.orderNumber = a.orderNumber)

#################################################################

SELECT a.months, a.productCode, a.quantityOrdered
FROM table1 AS a
WHERE NOT EXISTS
	(SELECT * FROM table1 AS b
    	WHERE b.months = a.months
    	AND b.quantityOrdered > a.quantityOrdered )
ORDER BY a.months

;

```
<br>

**Answer (Query result)**

| months | productCode | quantityOrdered |
|---|----------|----|
| 1 | S18_2248 | 50 |
| 1 | S18_2581 | 50 |
| 1 | S18_2795 | 50 |
| 1 | S24_1578 | 50 |
| 1 | S32_1374 | 50 |
| 1 | S18_3856 | 50 |
| 2 | S10_4757 | 50 |
| 2 | S24_3816 | 50 |
| 2 | S24_3949 | 50 |
| 2 | S18_4027 | 50 | 
| 2 | S50_4713 | 50 |
| 2 | S12_4675 | 50 |
| 2 | S18_3320 | 50 |
| 3 | S18_4668 | 50 |
| 3 | S10_4962 | 50 |
| 3 | S700_2824 | 50 |
| 3 | S18_1662 | 50 |
| 3 | S24_4278 | 50 |
| 3 | S72_1253 | 50 |
| 3 | S10_2016 | 50 |
| 3 | S700_2834 | 50 |
| 3 | S18_3482 | 50 |
| 4 | S12_4675 | 97 |
| 5 | S24_2300 | 70 |
| 5 | S24_3856 | 70 |
| 6 | S24_3816 | 50 |
| 6 | S24_3949 | 50 |
| 6 | S18_2949 | 50 |
| 6 | S700_3962 | 50 |
| 7 | S18_2325 | 50 |
| 7 | S18_2238 | 50 |
| 7 | S18_2319 | 50 |
| 7 | S18_3856 | 50 |
| 8 | S12_1099 | 50 |
| 8 | S18_1342 | 50 |
| 8 | S32_4289 | 50 |
| 9 | S18_1342 | 50 |
| 9 | S700_3505 | 50 |
| 9 | S72_3212 | 50 |
| 9 | S24_1628 | 50 |
| 9 | S32_2509 | 50 |
| 10 | S24_3371 | 50 |
| 10 | S12_4675 | 50 |
| 10 | S18_3259 | 50 |
| 10 | S24_1578 | 50 |
| 10 | S24_3856 | 50 |
| 10 | S700_1138 | 50 |
| 10 | S32_4485 | 50 |
| 11 | S24_2841 | 55 |
| 11 | S24_3151 | 55 |
| 11 | S700_2047 | 55 |
| 11 | S24_2000 | 55 |
| 11 | S32_1374 | 55 |
| 11 | S700_2466 | 55 |
| 11 | S12_2823 | 55 |
| 11 | S12_4675 | 55 |
| 11 | S18_1889 | 55 |
| 11 | S18_3482 | 55 |
| 12 | S24_2360 | 50 |
| 12 | S24_4620 | 50 |
| 12 | S18_1342 | 50 |
| 12 | S18_1662 | 50 |


# <h3>Q3 : Plot the total revenue for each month and year.</h3>
<br>

```ruby

USE classicmodels ;

SELECT 
	MONTH(a.orderDate) as months,
    	ROUND(SUM(CASE WHEN year(a.orderDate) = 2003 THEN b.total ELSE 0 END ),2) AS '2003' ,
    	ROUND(SUM(CASE WHEN year(a.orderDate) = 2004 THEN b.total ELSE 0 END ),2) AS '2004' ,
    	ROUND(SUM(CASE WHEN year(a.orderDate) = 2005 THEN b.total ELSE 0 END ),2) AS '2005' ,
	SUM(b.total) as totalRev
FROM orders AS a
INNER JOIN 
	(SELECT orderNumber, (quantityOrdered*priceEach) AS total
	FROM orderdetails) AS b
ON a.orderNumber = b.orderNumber
GROUP BY months

```
<br>

**Answer (Query result)**

| months | 2003 | 2004 | 2005 | totalRev |
|---|---|---|---|---|
| 1.0 | 116692.77 | 292385.21 | 307737.02 | 716815.0 |
| 2.0 | 128403.64 | 289502.84 | 317192.17 | 735098.65 |
| 3.0 | 160517.14 | 217691.26 | 359711.96 | 737920.36 |
| 4.0 | 185848.59 | 187575.77 | 344820.62 | 718244.98 |
| 5.0 | 179435.55 | 248325.3 | 441474.94 | 869235.79 |
| 6.0 | 150470.77 | 343370.74 | 0.0 | 493841.51 |
| 7.0 | 201940.36 | 325563.49 | 0.0 | 527503.85 |
| 8.0 | 178257.11 | 419327.09 | 0.0 | 597584.2 |
| 9.0 | 236697.85 | 283799.8 | 0.0 | 520497.65 |
| 10.0 | 514336.21 | 500233.86 | 0.0 | 1014570.07 |
| 11.0 | 988025.15 | 979291.98 | 0.0 | 1967317.13 |
| 12.0 | 276723.25 | 428838.17 | 0.0 | 705561.42 |

<br>

#

Go to top : [Top](https://github.com/HikariJadeEmpire/SQL_AdvanceQuery#advanced-sql-query)
