# Danny's Diner
Danny seriously loves Japanese food so in the beginning of 2021, he decides to embark upon a risky venture and opens up a cute little restaurant that sells his 3 favorite foods: sushi, curry and ramen.
Danny’s Diner is in need of your assistance to help the restaurant stay afloat - the restaurant has captured some very basic data from their few months of operation but have no idea how to use their data to help them run the business.

## Problem Statement
Danny wants to use the data to answer a few simple questions about his customers, especially about their visiting patterns, how much money they’ve spent and also which menu items are their favorite. Having this deeper connection with his customers will help him deliver a better and more personalized experience for his loyal customers.

He plans on using these insights to help him decide whether he should expand the existing customer loyalty program - additionally he needs help to generate some basic datasets so his team can easily inspect the data without needing to use SQL.

Danny has provided you with a sample of his overall customer data due to privacy issues - but he hopes that these examples are enough for you to write fully functioning SQL queries to help him answer his questions!

Danny has shared with you 3 key datasets for this case study:

sales  menu  members

![Screenshot (133)](https://github.com/bhammy27/SQL_Danny-s-Diner/assets/154477061/96a66685-7619-4dfd-a6c6-9bd1cb89002f)

## Analysis
### What is the total amount each customer spent at the restaurant?
- The menu table contains the price for each product. Lets join the sales and menu tables then sum the prices for the orders made by each customer after grouping by the customer_id. Lets also join the table with the USING clause vs joing by sales.product_id = menu.product_id.

![Screenshot (134)](https://github.com/bhammy27/SQL_Danny-s-Diner/assets/154477061/dc7c66e6-c1d1-4897-9d0c-e3163ae077bd)

  - Customer A, B and C spent $76, $74 and $36 respectively

### How many days has each customer visited the restaurant?
- Each order has an order date and a visiting customer can make more than one order in a day. So one visit can have multiple entries for the same date. Hence, we need to count just the unique order dates for each customer.

![Screenshot (135)](https://github.com/bhammy27/SQL_Danny-s-Diner/assets/154477061/71fa53eb-b460-4e12-9cb1-f1d5150fb883)

  - B had the most visits (6) while C had the least visits (2)

### What was the first item from the menu purchased by each customer?
- Using a Common Table Expression (CTE) creates a temporary result set. We will rank the orders by their order_date from the earliest to the latest for each customer using the DENSE_RANK function. The earliest order_date of each customer will get a rank of 1.
- We will then write another select statement to access the output of the CTE and limit what we display to just those records with a rank of 1.

![Screenshot (136)](https://github.com/bhammy27/SQL_Danny-s-Diner/assets/154477061/eddc77d9-fd0f-4450-bac5-9b086ebf37af)

  - Customer A’s first orders are curry and sushi.
  - Customer B’s first order is curry.
  - Customer C’s first order is ramen.

### What is the most purchased item on the menu and how many times was it purchased by all customers?
- We are going to join sales and menu tables so we can display the product name, group by product name and take count for each product. We will then order our products in descending order ( from the highest count to the lowest). Using LIMIT 1 clause, we can filter our results to show just the product with the highest count which is the product that appears on the first row.

![Screenshot (137)](https://github.com/bhammy27/SQL_Danny-s-Diner/assets/154477061/cab943de-3e9f-44dc-8afa-0b90b34d1089)

  - With 8 purchases, ramen is the most purchased item.

### Which item was the most popular for each customer?
- Take a count of the product ids after grouping by product and by customer.
- Use the DENSE_RANK function to rank products in descending order with respect to their product counts for each customer.
- The products with the rank of 1 will represent the most popular products for each customer.

![Screenshot (138)](https://github.com/bhammy27/SQL_Danny-s-Diner/assets/154477061/b6df1a42-35ae-4615-9c39-423df4d4b97f)![Screenshot (139)](https://github.com/bhammy27/SQL_Danny-s-Diner/assets/154477061/37ae19e1-b24a-449b-9359-4d270b5b4425)

  - Customer A and C’s favourite item is ramen.
  - Customer B enjoys sushi, ramen and curry equally.

### Which item was purchased first by the customer after they became a member?
- We want to find purchases made after the join dates in the members table. To achieve this we will join the sales and members table using customer_id. Then filter the results using a WHERE clause to find only purchases made on or after the respective join dates for each member.
- Using the DENSE_RANK, we willrank  order dates for each customer from the earliest to the latest.
- The records with a rank of 1 will represent the earliest purchases made by each member

![Screenshot (140)](https://github.com/bhammy27/SQL_Danny-s-Diner/assets/154477061/c055080d-9970-4b2b-b655-8defdb1df552)

  - After becoming members, tCustomer A and Customer B first purchased curry and sushi respectively.
### Which item was purchased just before the customer became a member?
-  Our approach will be similar to question 6 but with two key differences :

  - We will be looking for order dates before the respective member join dates and

  - This time we will rank the order dates in descending order from latest to the earliest.

- The records with a rank of 1 will now represent orders made just before a customer became a member.

![Screenshot (141)](https://github.com/bhammy27/SQL_Danny-s-Diner/assets/154477061/9a65c87b-f7a3-4d95-84b0-0dd36903bea1)

  - Just before becoming members, Customer A bought sushi and curry while Customer B bought sushi.

### What is the total items and amount spent for each member before they became a member?
- To solve this, we will need the prices of items in sales table. This can be found in the menu table and the join dates which can be found in the members table. Hence, we will be making 2 joins. After which we will apply a filter to limit the results to purchases made before the respective join dates of each member.

![Screenshot (142)](https://github.com/bhammy27/SQL_Danny-s-Diner/assets/154477061/10914ee2-89cd-4946-86d0-499409d34b76)

  - Before becoming members, Customer A had spent $25 on 2 items and Customer B, $40 on 2 items.

### If each $1 spent equates to 10 points and sushi has a 2x points multiplier, how many points would each customer have?
- We will need create a new column to register points earned by customers. The values in this new column will be calculated using a CASE statement to allocate points depending on whether sushi or some other item was purchased.

![Screenshot (143)](https://github.com/bhammy27/SQL_Danny-s-Diner/assets/154477061/98c0e11c-99ab-4a82-91aa-31559bc655ac)

  - A got 860 points, B got 940 points and C got 360 points.

### In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi — how many points do customer A and B have at the end of January?
- This question introduces two extra conditions to the previous scenario in question 9. We have to differentiate purchases made within a week after joining as a member from the rest and limit our calculations to just January of that year.
- The difference in days between the last day and the first day of a week is 6. If the purchase was made on the day he joined the difference will 0 and if it was made on last day of that week it will be 6. So we need find purchases made between these intervals (0 & 6)
- We can confirm if a date falls in January 2021 by using the EXTRACT function to extract the month and the year from it. January’s numeric equivalent is 1 being that it’s the first month.

![Screenshot (144)](https://github.com/bhammy27/SQL_Danny-s-Diner/assets/154477061/dceeb1f7-5450-40e8-85e0-a14188d60a24)![Screenshot (145)](https://github.com/bhammy27/SQL_Danny-s-Diner/assets/154477061/88c24a31-387c-4497-a805-7c53dcad6dee)

  - A got 1370 points and B got 820 points.
## Making A Comprehensive Customer Data Table
- Here we need to create a view. A view is a virtual table. A view does not save the table data. It saves the query so that you do not need to rewrite it every time you need to retrieve data. You can view it like a normal table while in the background the saved query is called. An important advantage of the view is that when the source table(s) updates, the view also updates to show us the most recent changes.

![Screenshot (146)](https://github.com/bhammy27/SQL_Danny-s-Diner/assets/154477061/e73dc59b-6760-4e86-85f4-159d58a0f484)![Screenshot (147)](https://github.com/bhammy27/SQL_Danny-s-Diner/assets/154477061/5825e1c5-0a38-4f80-882c-db07df2cc035)

## Ranking Member Customer Products:
Danny also requires further information about the ranking of customer products, but he purposely does not need the ranking for non-member purchases so he expects null ranking values for the records when customers are not yet part of the loyalty program.

- We are going to use the CASE statement to create another column that will substitute [null] for ’N’ then rank the other rows that have ‘Y’.

![Screenshot (148)](https://github.com/bhammy27/SQL_Danny-s-Diner/assets/154477061/6e028fd6-c679-4f79-9bad-b6fd0e68cb1f)

