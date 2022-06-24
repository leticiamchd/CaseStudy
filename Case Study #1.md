**Link** -> https://8weeksqlchallenge.com/case-study-1/

# Questions

**1 - What is the total amount each customer spent at the restaurant?**

```sql
SELECT customer_id,
    SUM(price) AS total_amount
FROM dannys_diner.sales sales
    INNER JOIN dannys_diner.menu menu
    ON sales.product_id = menu.product_id
GROUP BY customer_id
ORDER BY total_amount DESC
``` 

**ANSWER:**
customer_ id	total_amount
A		76		
B		74


**2 - How many days has each customer visited the restaurant?**

```sql
SELECT customer_id, 
	COUNT(order_date) AS days_visited
FROM dannys_diner.sales 
GROUP BY customer_id
ORDER BY customer_id
``` 

**ANSWER:**
customer_ id	days_visited
A		6		
B		6

**3 - What was the first item from the menu purchased by each customer?**

```sql
 SELECT 
	customer_id, order_date, product_name
FROM dannys_diner.sales sales
	INNER JOIN dannys_diner.menu menu
    ON sales.product_id = menu.product_id
WHERE order_date = '2021-01-01'
``` 

**ANSWER:**
customer_ id	order_date	product_name
A		2021-01-01	sushi		
A		2021-01-01	curry
B		2021-01-01	curry
C		2021-01-01	ramen

**4 - What is the most purchased item on the menu and how many times was it purchased by all customers?**

```sql
SELECT 
    product_name, 
    COUNT(product_name) AS favorite
FROM dannys_diner.menu menu
	INNER JOIN dannys_diner.sales sales
    ON menu.product_id = sales.product_id
GROUP BY product_name
ORDER BY favorite DESC
``` 

**ANSWER:**
product_name	favorite
ramen		8
curry		4
sushi		3 		

**5 - Which item was the most popular for each customer?**

```sql
SELECT 
	customer_id,
    product_name, 
    COUNT(product_name) AS favorite
FROM dannys_diner.sales sales
	INNER JOIN dannys_diner.menu menu
    ON sales.product_id = menu.product_id
GROUP BY customer_id, product_name
ORDER BY favorite DESC
``` 

**ANSWER:**
customer_ id	product_name	favorite
A		ramen		3		
C		ramen		3	
A		curry		2
B		curry		2

**6 - Which item was purchased first by the customer after they became a member?**

```sql
SELECT 
	sales.customer_id,
    product_name, 
    order_date
FROM dannys_diner.sales sales
	INNER JOIN dannys_diner.menu menu
    ON sales.product_id = menu.product_id
    INNER JOIN dannys_diner.members members
    ON members.customer_id = sales.customer_id
WHERE order_date >= join_date
ORDER BY order_date
 ``` 
 
 **7 - Which item was purchased just before the customer became a member?**

```sql
 SELECT 
	sales.customer_id,
    product_name, 
    order_date
FROM dannys_diner.sales sales
	INNER JOIN dannys_diner.menu menu
    ON sales.product_id = menu.product_id
    INNER JOIN dannys_diner.members members
    ON members.customer_id = sales.customer_id
WHERE order_date < join_date
ORDER BY order_date DESC
``` 

**8 - What is the total items and amount spent for each member before they became a member?**

```sql
SELECT 
	sales.customer_id,
    COUNT(product_name) AS total_items,
    SUM(price) AS total_amount
FROM dannys_diner.sales sales
	INNER JOIN dannys_diner.menu menu
    ON sales.product_id = menu.product_id
    INNER JOIN dannys_diner.members members
    ON members.customer_id = sales.customer_id
WHERE order_date < join_date
GROUP BY sales.customer_id
``` 

**9 - If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have? **

```sql
SELECT 
	customer_id, 
    SUM(CASE 
    	WHEN product_name = 'sushi' THEN price * 20
      	ELSE price * 10
  		END) AS points

FROM dannys_diner.sales sales
INNER JOIN dannys_diner.menu menu
  ON sales.product_id = menu.product_id
  
GROUP BY customer_id
ORDER BY customer_id
``` 

**10 - In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?**

**OBS** THIS WAS MADE ON BIGQUERY 

DOUBTS: THE POINTS START TO COUNT JUST AFTER THEY BECOME MEMBERS ?

```sql
WITH all_data AS
(
  SELECT sales.customer_id, order_date, sales.product_id,join_date,
  product_name, price
  FROM
   `resonant-cairn-350019.case_study.sales` as sales 
   INNER JOIN `resonant-cairn-350019.case_study.menu` as menu
   ON sales.product_id = menu.product_id
   INNER JOIN `resonant-cairn-350019.case_study.members` as members
   ON sales.customer_id = members.customer_id
)

SELECT 
	customer_id, 
    SUM(CASE 
    	    WHEN DATE_DIFF(order_date, join_date, DAY) <= 7 THEN price * 20
          WHEN DATE_DIFF(order_date, join_date, DAY) > 7 AND product_name = 'sushi' THEN price * 20
      	  ELSE price * 10
  		END) AS points

FROM all_data
WHERE order_date >= join_date AND order_date < '2021-02-01'
GROUP BY customer_id
ORDER BY customer_id
``` 
# Bonus Questions 

**11 - Join All the Things**

```sql
WITH all_data AS
(
  SELECT sales.customer_id, order_date, sales.product_id,join_date,
  product_name, price,
  FROM
   `resonant-cairn-350019.case_study.sales` as sales 
   INNER JOIN `resonant-cairn-350019.case_study.menu` as menu
   ON sales.product_id = menu.product_id
   INNER JOIN `resonant-cairn-350019.case_study.members` as members
   ON sales.customer_id = members.customer_id
)

SELECT 
	customer_id,order_date, product_name, price,
  CASE
    WHEN customer_id = 'A' AND order_date >= '2021-01-07' THEN 'Y'
    WHEN customer_id = 'B' AND order_date >= '2021-01-09' THEN 'Y'
    ELSE 'N'
    END AS member
 

FROM all_data
ORDER BY customer_id, order_date, product_name
``` 
