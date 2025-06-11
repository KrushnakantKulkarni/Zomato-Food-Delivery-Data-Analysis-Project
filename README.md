# Zomato-Food-Delivery-Data-Analysis-Project
This repository contains SQL-based analysis for Zomato's food delivery data. The project is designed to extract business insights, customer behavior, operational performance, and delivery efficiency from the dataset using various SQL queries.
🗂️ Table of Contents
📌 Overview

🧮 Database Schema

📋 Business Questions Solved

📦 Dataset Structure

🔧 SQL Queries

🚀 Getting Started

📎 License

📌 Overview
This project focuses on analyzing a Zomato-style food delivery system. Using SQL queries, it dives into:

Delivery and cancellation performance

Restaurant and cuisine ratings

Revenue analysis

Customer behavior

Delivery partner performance

🧮 Database Schema
The analysis is based on a single comprehensive table:

zomato_food_delivery

| Column Name               | Data Type        | Description                               |
| ------------------------- | ---------------- | ----------------------------------------- |
| date                      | DATE             | Date of the order                         |
| time                      | TIME             | Time of the order                         |
| order\_id                 | VARCHAR(20) (PK) | Unique ID for each order                  |
| order\_status             | VARCHAR(20)      | Delivered / Cancelled / Incomplete        |
| customer\_id              | VARCHAR(20)      | ID of the customer                        |
| restaurant\_name          | VARCHAR(100)     | Name of the restaurant                    |
| cuisine\_type             | VARCHAR(50)      | Cuisine offered                           |
| pickup\_location          | VARCHAR(100)     | Restaurant's location                     |
| delivery\_location        | VARCHAR(100)     | Customer's delivery location              |
| estimated\_prep\_time     | INT              | Estimated preparation time in minutes     |
| actual\_prep\_time        | INT              | Actual preparation time in minutes        |
| delivery\_time\_mins      | INT              | Delivery time (null if not delivered)     |
| cancelled\_by\_customer   | VARCHAR(3)       | Yes / No                                  |
| reason\_cancel\_customer  | VARCHAR(100)     | Reason for customer cancellation          |
| cancelled\_by\_partner    | VARCHAR(3)       | Yes / No                                  |
| reason\_cancel\_partner   | VARCHAR(100)     | Reason for partner cancellation           |
| incomplete\_orders        | VARCHAR(3)       | Yes / No                                  |
| incomplete\_reason        | VARCHAR(100)     | Reason for incompleteness                 |
| order\_value\_inr         | DECIMAL(10, 2)   | Original order value                      |
| discount\_applied\_inr    | DECIMAL(10, 2)   | Discount amount applied                   |
| final\_amount\_paid\_inr  | DECIMAL(10, 2)   | Final amount paid (null if not delivered) |
| payment\_method           | VARCHAR(20)      | UPI / Card / Cash / Wallet                |
| delivery\_partner\_rating | FLOAT            | Rating for delivery partner               |
| customer\_rating          | FLOAT            | Customer's order rating                   |
| delivery\_partner\_id     | VARCHAR(20)      | ID of the delivery partner                |


📋 Business Questions Solved
✅ Retrieve all delivered orders

⏱️ Average delivery time by cuisine type

❌ Total cancellations by customers

🍽️ Top 5 restaurants with most delivered orders

🚚 Cancellation reasons by delivery partners

⭐ Max and min customer ratings for North Indian cuisine

💳 Orders paid through Wallet

🛵 Average delivery partner rating per restaurant

💰 Total revenue from delivered orders

❗ Incomplete orders with reasons

📉 Most common customer cancellation reasons

🗺️ Top delivery location by delivered orders

⏱️ Orders where actual prep time > estimated time

💸 Avg discount by cuisine type

😞 Customers with multiple 1-star ratings

💼 Payment method distribution for delivered orders

💵 Orders with final amount paid > ₹800

🥇 Best-rated restaurant by customers

📅 Daily order count where order value > ₹500

🏍️ Delivery partners with avg rating > 4.5
```

--✅ SQL Questions – Zomato Data Analyst Project
--01.Retrieve all delivered orders.
select * from zomato_food_delivery 
where order_status = 'Delivered'; 

--02.Find the average delivery time for each cuisine type.
select
	cuisine_type,
	avg(delivery_time_mins) as avg_time
from zomato_food_delivery
group by cuisine_type ;

--03.Get the total number of orders cancelled by customers.
select 
	count(*) as count_cancelled_orders 
from zomato_food_delivery
where cancelled_by_customer ='Yes' ;

--04.List the top 5 restaurants with the most delivered orders.
select 
	restaurant_name,
	count(order_id) as orders_count 
from zomato_food_delivery
group by restaurant_name
order by orders_count desc 
limit 5 ;

--05.Get the number of cancellations by delivery partners, grouped by reason.
select
	reason_cancel_partner,
	count(*) 
from zomato_food_delivery
where cancelled_by_partner ='Yes' 
group by reason_cancel_partner

--06.Find the highest and lowest customer ratings for North Indian cuisine.

select
	max(customer_rating) as max_ratings,
	min(customer_rating) as min_ratings
from zomato_food_delivery 
where cuisine_type = 'North Indian' ;

--07.Retrieve all orders paid using Wallet.
select * from zomato_food_delivery
where payment_method = 'Wallet' ;

--08.Find the average delivery partner rating for each restaurant.
select 
	restaurant_name,
	avg(delivery_partner_rating) as avg_partner_rating
from zomato_food_delivery
group by restaurant_name ;

--09.Calculate the total revenue generated from successfully delivered orders.
select sum(final_amount_paid_inr) from zomato_food_delivery
where order_status = 'Delivered'; 

--10.List all incomplete orders along with the reason.
select incomplete_reason,order_id 
from zomato_food_delivery
where incomplete_orders = 'Yes';

--11.Get the most common reason for customer cancellations.

select 
	reason_cancel_customer,
	count(*) as count
from zomato_food_delivery
where cancelled_by_customer ='Yes'
group by reason_cancel_customer 
order by count desc ;


--Find out which delivery location had the highest number of delivered orders.
select 
	delivery_location,
	count(*) as order_count
from zomato_food_delivery
group by delivery_location 
order by order_count desc ;

--Count how many orders had actual prep time more than estimated prep time.
select order_id from zomato_food_delivery
where estimated_prep_time < actual_prep_time ;

--Get the average discount applied per cuisine type.
select 
	cuisine_type,
	avg(discount_applied_inr) as avg_discount_ammount
from zomato_food_delivery
group by cuisine_type ;

--Identify customers who gave the lowest rating (1) more than once.
select customer_id,count(*) as rating_count from zomato_food_delivery
WHERE customer_rating = 1
group by customer_id
having count(*) > 1 ;

SELECT customer_id, COUNT(*) AS rating_count 
FROM zomato_food_delivery
WHERE customer_rating = 1
GROUP BY customer_id
HAVING COUNT(*) > 1;

--Show the distribution of payment methods for delivered orders.
select payment_method,count(*) as payment_count
from zomato_food_delivery
where order_status = 'Delivered'
group by payment_method

--List orders where final amount paid is greater than ₹800.
select order_id, final_amount_paid_inr from zomato_food_delivery
where final_amount_paid_inr > 800 ;

--Find the restaurant with the best average customer rating.
select restaurant_name, avg(customer_rating) as avg_customer_ratings
from zomato_food_delivery
group by restaurant_name
order by avg_customer_ratings desc 

--Count orders per day where order value was greater than ₹500.
select date,count(*) as count_orders
from zomato_food_delivery
where order_value_inr > 500 
group by date;

--List delivery partners who received an average rating above 4.5.
select delivery_partner_id,AVG(delivery_partner_rating) as avg_ratings
from zomato_food_delivery
WHERE order_status = 'Delivered'
GROUP BY delivery_partner_id
HAVING AVG(delivery_partner_rating) > 4.5;


```

📦 Dataset Structure
Single Table with detailed order data

Focus Areas: Delivery performance, customer feedback, sales, cancellations

🔧 SQL Queries
All queries are written in standard SQL and can run in MySQL, PostgreSQL, or SQLite with minimal adjustments.

Includes:

Filtering, Grouping, Aggregations

Date and Time functions

NULL handling

Conditional logic

