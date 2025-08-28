# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Database**: `project1`
This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `project1`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE project1;

--TABLE CREATION 
create table retail_sales(
transactions_id int primary key, 
sale_date date,
sale_time time,
customer_id int,
gender varchar(10),
age int,
category varchar(20),
quantiy int,
price_per_unit float,
cogs float,
total_sale float
)

select * from retail_sales;

--FINDING NULL VALUES:-
select * from retail_sales
where transactions_id is null;

select * from retail_sales
where sale_date is null;

select * from retail_sales
where 
transactions_id is null
or 
sale_date is null
or
sale_time is null
or
customer_id is null
or
gender is null
or
age is null
or
category is null
or
quantiy is null
or 
price_per_unit is null
or 
cogs is null
or
total_sale is null;

--DATA CLEANING (DELETING NULL VALUES):-
delete from retail_sales 
where 
transactions_id is null
or 
sale_date is null
or
sale_time is null
or
customer_id is null
or
gender is null
or
age is null
or
category is null
or
quantiy is null
or 
price_per_unit is null
or 
cogs is null
or
total_sale is null;

--DATA EXPLORATION :-

	-- How many sales we have?
	select count(*) as total_sales from retail_sales;

	-- How many customers we have?
	select count(distinct customer_id) as total_customers from retail_sales;

	--Which categories we have?
	select distinct category from retail_sales;

--DATA ANALYSIS:-

	--Retrieve all columns for sales made on '2022-11-05'
	select * from retail_sales 
	where sale_date = '2022-11-05';

	--Retrieve all transactions where the category is 'Clothing' and the quantity sold is more than or equal to 4 in the month of Nov-2022:
	select * from retail_sales
	where category = 'Clothing'
	AND 
	to_char(sale_date ,'yyyy-mm') = '2022-11'
	AND 
	quantiy >= 4;

	--Calculate total sales for each category
	select category , sum(total_sale) as net_sale
	from retail_sales
	group by category;

	--Find avg age of customers who purchaswed items from 'beauty' category
	select avg(age)::int as avg_age 
	from retail_sales
	where category = 'Beauty';

	--Find all trsnsactions where total sale is more than 1000
	select * from retail_sales 
	where total_sale > 1000;

	--Finf total number of transactions(trans_id) made by each gender in each category
	select category,gender,count(*) from retail_sales 
	group by gender , category;

	--Calculate avg sale for each month and find the best selling month in each year
	select
	date_part('year',sale_date) as year,
	date_part('month',sale_date) as month,
	round(avg(total_sale)) as avg_sale,
	rank() over(partition by date_part('year',sale_date)order by round(avg(total_sale)) desc ) as rank
	from retail_sales
	group by 1,2;

	--Find top 5 customers based on highest total sales
	select customer_id , SUM(total_sale) 
	from retail_sales
	group by 1
	order by 2 desc 
	limit 5;

	--Find number of unique customers who purchased items for each category
	select count(distinct customer_id),category 
	from retail_sales
	group by 2;

	--Create each shift and no. of orders for them
	with hourly_orders
	as
	(
	select transactions_id, 
	sale_time,
	case 
	when extract (hour from sale_time) < '12'
	then 'morning' 
	when extract (hour from sale_time) between '12' and '17'
	then 'afternoon'
	when extract (hour from sale_time) > '17'
	then 'evening' 
	end as shift
	from retail_sales
	)
	select shift , 
	count(*) order_hours
	from hourly_orders 
	group by shift;
	
	

## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts provided in the `database_setup.sql` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `analysis_queries.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

## Author - Rohan Khatri

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch!

Thank you for your support, and I look forward to connecting with you!

