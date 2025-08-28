## Customer Sale Insights (using MySQL Workbench)

### Goal:

This repo is to analyze a dataset from a tech store which include 3 csv files (**product.csv**, **customer.csv** & **order.csv**)

The dataset EER diagram is as follows:

![This is an alt text.](https://github.com/DongVND/SQL-project-1/blob/main/sales%20analyst%20eer%20diagram.png)

**Note: The database and tables are imported and populated in a local server*

We will answer these questions:
1. What is the total for each category?
2. How many orders did each customer make?
3. How is the sale in each region?
4. Which customer segment brings the highest profit
5. Which product is the most profitable one?
6. What is the total sale per customer by product categories?


### Progression in details:
#### Create database and populate the tables:

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


#### Query to find the answers

##### 1. What is the total for each category?
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

|region|total_sales|sales_rank|
|------|-----------|----------|
|West  |2900       |1         |
|North |1770       |2         |
|East  |1675       |3         |
|South |1170       |4         |

4. Which customer segment brings the highest profit
5. Which product is the most profitable one?
6. What is the total sale per customer by product categories?
