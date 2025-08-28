## Customer Sale Insights (using MySQL Workbench)

### Goal:

This repo is to analyze a dataset from a tech store which include 3 csv files (**product.csv**, **customer.csv** & **order.csv**)

The dataset EER diagram is as follows:

![This is an alt text.](https://github.com/DongVND/SQL-project-1/blob/main/sales%20analyst%20diagram.png)

**Note: The database and tables are imported and populated in a local server*

We will answer these questions:
1. What is the total for each category?
2. How many orders did each customer make?
3. How is the sale in each region?
4. What are the Top 5 customer segments that bring the highest profit?
5. Which products are the top profitable ones in each category?
6. What are the Top 5 customers with the highest sales (and more than 500) last year?


### Progression in details:
#### STEP 1: Create database and populate the tables:

Create a database called 'sales'
```
create database sales;
use sales;
```

Create tables named 'order', 'product', and 'customer' according to the csv files.

Table **order**:
```
create table orders (
orderid varchar(50) primary key,
orderdate date,
customerid varchar(50),
region varchar(50), 
productid varchar(50),
sale float,
profit float,
discount float, 
quantity int, 
category varchar(50));
```
Table **product**:
```
create table product (
product_id varchar(50) primary key,
product_name varchar(50),
category varchar(50),
sub_category varchar(50));
```
Table **customer**:
```
create table customer (
customer_id varchar(50) primary key,
customer_name varchar(50),
segment varchar(50));
```

*The record from csv files should be imported into the created table by using SQL Workbench Table Data Import Wizard.*

---


#### STEP 2: Query to find the answers

##### 1. What is the total sale for each category?
```
SELECT category, SUM(sales) AS total_sales
FROM orders
GROUP BY category;
```
Result:
|category       |total_sales|
|---------------|-----------|
|Technology     |4670       |
|Furniture      |1800       |
|Office Supplies|1045       |

##### 2. How many orders did each customer make?

```
SELECT  customerid, COUNT(orderid)
FROM orders
GROUP BY customerid;
```
Result:
|customerid     |COUNT(orderid)|
|---------------|--------------|
|CUST001        |4             |
|CUST002        |3             |
|CUST003        |4             |
|CUST004        |2             |
|CUST005        |2             |
|CUST006        |2             |
|CUST007        |3             |
|CUST008        |2             |
|CUST009        |1             |
|CUST010        |2             |

##### 3. How is the sale in each region?
```
SELECT 
region, SUM(sales) AS total_sales, RANK() OVER(ORDER BY SUM(sales) DESC) AS sales_rank
FROM orders
GROUP BY region;
```
Result:
|region|total_sales|sales_rank|
|------|-----------|----------|
|West  |2900       |1         |
|North |1770       |2         |
|East  |1675       |3         |
|South |1170       |4         |

##### 4. Top 5 customer segments bring the highest profit
```
SELECT 
customer.customerid, 
customer.segment, 
SUM(profit) AS total_profit
FROM customer
JOIN orders ON customer.customerid= orders.customerid
GROUP BY customerid, segment
ORDER BY total_profit DESC
LIMIT 5;
```
Result:
|customerid|segment|total_profit|
|----------|-------|------------|
|CUST002   |Corporate|250         |
|CUST010   |Consumer|250         |
|CUST003   |Consumer|200         |
|CUST007   |Corporate|200         |
|CUST004   |Corporate|170         |


##### 5. Which products are the top profitable ones in each category?

```
SELECT 
orders.category,
productname,
SUM(sales) AS total_sales,
RANK() OVER(PARTITION BY category ORDER BY SUM(sales) DESC) AS sale_rank

FROM orders JOIN product 
ON orders.productid = product.productid

GROUP BY category, productname
ORDER BY category , sale_rank;
```
Result:
|category|productname|total_sales|sale_rank|
|--------|-----------|-----------|---------|
|Furniture|Desk       |900        |1        |
|Furniture|Office Chair|450        |2        |
|Furniture|File Cabinet|450        |2        |
|Office Supplies|Notebook   |950        |1        |
|Office Supplies|Pen        |95         |2        |
|Technology|Monitor    |1850       |1        |
|Technology|Printer    |1450       |2        |
|Technology|Laptop     |750        |3        |
|Technology|Smartphone |350        |4        |
|Technology|Keyboard   |270        |5        |


##### 6. TOP 5 customers with the highest sales (and more than 500) last year ?
```
SELECT 
orders.customerid,
customer.customername,
orders.category,
SUM(sales) AS total_sales
FROM orders JOIN customer 
ON orders.customerid = customer.customerid

 WHERE  orderdate BETWEEN '01/01/2024' AND '12/31/2024'
 GROUP BY  customerid, customername, category
 
 HAVING SUM(sales)>500
 ORDER BY total_sales DESC
 LIMIT 5;
```
Result:
|customerid|customername|category|total_sales|
|----------|------------|--------|-----------|
|CUST002   |Jane Smith  |Technology|700        |
|CUST003   |Bob Johnson |Technology|670        |
|CUST007   |Michael King|Technology|650        |
|CUST009   |Patricia Davis|Technology|600        |

