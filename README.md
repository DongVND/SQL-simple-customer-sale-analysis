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











