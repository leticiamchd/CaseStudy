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

