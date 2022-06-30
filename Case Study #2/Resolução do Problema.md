# Cleaning Data 

#### Tabela customer_orders

```sql
with clean_customer_orders as
( 
      SELECT order_id, customer_id, pizza_id,
             (case when exclusions is null or exclusions like '%null%' OR exclusions = "NaN" then '0'
                  else exclusions 
             end) as exclusions,
             (case when extras is null or extras like '%null%' OR extras = "NaN" then '0' 
                  else extras 
             end) as extras, 
             order_time 
      from `resonant-cairn-350019.case_study2.customer_orders`
 ) 
 select * 
 from clean_customer_orders
 ````
 
 ##### Tabela runer_orders
 
```sql
with clean_runner_orders as
( 
      SELECT order_id, runner_id, 
             (case when pickup_time is null or pickup_time like '%null%' OR pickup_time = "NaN" then '0'
                  else pickup_time 
             end) as pickup_time,
             (case when distance is null or distance like '%null%' OR distance = "NaN" then '0' 
                  else distance 
             end) as distance, 
             (case when duration is null or duration like '%null%' OR duration = "NaN" then '0' 
                  else duration 
             end) as duration,
             (case when cancellation is null or cancellation like '%null%' OR cancellation = "NaN" then '0' 
                  else cancellation 
             end) as cancellation
      from `resonant-cairn-350019.case_study2.runner_orders`
 ) 
 select * 
 from clean_runner_orders
 ```







# A. Pizza Metrics

#### 1.

```sql
SELECT COUNT(pizza_id) AS pizzas_ordered

FROM `resonant-cairn-350019.case_study2.customer_orders`
```

#### 2. 

```sql
SELECT customer_id
FROM `resonant-cairn-350019.case_study2.customer_orders`

GROUP BY customer_id
````

**RESPOSTA**
| customer_id  |
| ------------ |
| 101  |
| 102  |
| 103  |
| 104  |
| 105  |

#### 3. 

```sql
with clean_runner_orders as
( 
      SELECT order_id, runner_id, 
             (case when pickup_time is null or pickup_time like '%null%' OR pickup_time = "NaN" then '0'
                  else pickup_time 
             end) as pickup_time,
             (case when distance is null or distance like '%null%' OR distance = "NaN" then '0' 
                  else distance 
             end) as distance, 
             (case when duration is null or duration like '%null%' OR duration = "NaN" then '0' 
                  else duration 
             end) as duration,
             (case when cancellation is null or cancellation like '%null%' OR cancellation = "NaN" then '0' 
                  else cancellation 
             end) as cancellation
      from `resonant-cairn-350019.case_study2.runner_orders`
 ) 
 select 
  COUNT(order_id) AS successful_orders
 from clean_runner_orders
 WHERE cancellation = '0'
 ```
 
 **RESPOSTA**
 | successful_orders  |
| ------------ |
|  8 |

#### 4. 

```sql
with clean_runner_orders as
( 
      SELECT order_id, runner_id, 
             (case when pickup_time is null or pickup_time like '%null%' OR pickup_time = "NaN" then '0'
                  else pickup_time 
             end) as pickup_time,
             (case when distance is null or distance like '%null%' OR distance = "NaN" then '0' 
                  else distance 
             end) as distance, 
             (case when duration is null or duration like '%null%' OR duration = "NaN" then '0' 
                  else duration 
             end) as duration,
             (case when cancellation is null or cancellation like '%null%' OR cancellation = "NaN" then '0' 
                  else cancellation 
             end) as cancellation
      from `resonant-cairn-350019.case_study2.runner_orders`
 ) 


 SELECT pizza_id, 
  COUNT(pizza_id) AS amount_pizza_delivered
FROM `resonant-cairn-350019.case_study2.customer_orders` AS customer
INNER JOIN clean_runner_orders AS runner
ON customer.order_id = runner.order_id
WHERE cancellation = '0'
GROUP BY pizza_id
````

**RESPOSTA**

| pizza_id  | amount_pizza_delivered  |
| ------------ | ------------ |
| 1  | 9  |
| 2  | 3  |

#### 5. 

```sql
SELECT customer_id, pizza_name,
COUNT(pizza_name) AS amount_pizza

FROM `resonant-cairn-350019.case_study2.customer_orders` AS customer
INNER JOIN `resonant-cairn-350019.case_study2.pizza_names` pizza
ON customer.pizza_id = pizza.pizza_id

GROUP BY customer_id, pizza_name
ORDER BY customer_id
```

**RESPOSTA**

| customer_id  | pizza_name  | amount_pizza  |
| ------------ | ------------ | ------------ |
| 101  |  Meat Lovers |  2 |
| 101  |  Vegetarian |  1 |
| 102  |  Meat Lovers |  2 |
|  102 |  Vegetarian |  1 |
| 103  |  Meat Lovers |  3 |
| 103 |  Vegetarian |  1 |
| 104  | Meat Lovers  | 3  |
|  105 | Vegetarian  |  1 |

#### 6. 

```sql 
SELECT customer_id,order_id,
COUNT(order_id) AS amount_order

FROM `resonant-cairn-350019.case_study2.customer_orders` 

GROUP BY customer_id, order_id
ORDER BY amount_order DESC
```
**RESPOSTA**

|  customer_id | order_id | amount_order   |
| ------------ | ------------ | ------------ |
| 103  |  4 | 3  |
|  102 | 3  | 2  |
| 104  | 10  | 2  |
| 101  | 1  |  1 |
| 101  | 2  |  1 |
|  103 |  9 |  1 |
| 104  | 5  | 1  |
| 101  | 6  | 1  |
|  105 |  7 |  1 |
|  102 |  8 |  1 |

####7. 



