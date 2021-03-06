# Resolução do Problema

Neste Notebook terá a resolução do Estudo de Caso número 1, também chamado de Restaurante do Danny. O desafio original se encontra todo em inglês, porém além de colocar as resoluções, deixarei todas as informações traduzidas. Lembrando que as variáveis se encontram na forma original (em inglês). 

**Link Original** -> https://8weeksqlchallenge.com/case-study-1/

## Introdução

Danny adora comida japonesa, então, no início de 2021, ele decidiu embarcar em um empreendimento arriscado e abriu um restaurante bonitinho que vende suas 3 comidas favoritas: sushi, curry e ramen.

O Restaurante do Danny precisa de sua ajuda para amanter o restaurante - o restaurante capturou alguns dados muito básicos de seus poucos meses de operação, mas não tem ideia de como usar seus dados para ajudá-los a administrar o negócio.

## O Problema

Danny quer usar os dados para responder a algumas perguntas simples sobre seus clientes, especialmente sobre seus padrões de visitas, quanto dinheiro eles gastaram e também quais itens do menu são seus favoritos. Ter essa conexão mais profunda com seus clientes o ajudará a oferecer uma experiência melhor e mais personalizada.

Ele planeja usar esses insights para ajudá-lo a decidir se deve expandir o programa de fidelidade do cliente existente - além disso, ele precisa de ajuda para gerar alguns conjuntos de dados básicos para que sua equipe possa inspecionar facilmente os dados sem precisar usar SQL.

Danny forneceu a você uma amostra de seus dados gerais de clientes devido a problemas de privacidade - mas ele espera que esses exemplos sejam suficientes para você escrever queries no SQL totalmente funcionais para ajudá-lo a responder suas perguntas!

Danny compartilhou com você 3 conjuntos de dados importantes para este estudo de caso:

> sales 
> 
> menu
> 
> members

Você pode inspecionar o diagrama da relação entre os conjuntos de dados do exemplo abaixo:

<img width="605" alt="Captura de Tela 2022-06-29 às 13 53 47" src="https://user-images.githubusercontent.com/66830501/176503776-3af5eb65-70ae-4cb6-838e-eb6d3d275f6b.png">


## Exemplo dos Conjuntos de Dados

Todos os conjuntos de dados existem no banco de dados chamado `dannys_diner` - certifique-se de incluir essa referência em seus scripts SQL ao começar a explorar os dados e responder às perguntas do estudo de caso.

#### Tabela 1: sales 

A tabela de `sales` captura todas as compras de acordo com o `customer_id`, com as informações de `order_date` e `product_id` correspondentes para quando e quais itens do menu foram pedidos.

<img width="391" alt="Captura de Tela 2022-06-29 às 14 03 20" src="https://user-images.githubusercontent.com/66830501/176503836-02a5fb9f-6ab9-49e6-bd8d-34c143a41383.png">

#### Tabela 2: menu

A tabela de `menu` mapeia o `product_id` para o `product_name` e o `price` de cada item do menu.

<img width="375" alt="Captura de Tela 2022-06-29 às 14 04 43" src="https://user-images.githubusercontent.com/66830501/176503871-75c9b873-0fda-4007-85be-3dcb3f06b9c9.png">

#### Tabela 3: members

A tabela `members` captura a `join_date` quando um `customer_id` ingressou na versão beta do programa de fidelidade do Restaurante do Danny.

<img width="273" alt="Captura de Tela 2022-06-29 às 14 04 51" src="https://user-images.githubusercontent.com/66830501/176503888-8505cd18-6ffb-42d0-ac20-beea5472f62c.png">

## Questões 

Cada uma das seguintes perguntas do estudo de caso pode ser respondida usando uma única instrução SQL:

1. Qual é o valor total que cada cliente gastou no restaurante?
2. Quantos dias cada cliente visitou o restaurante?
3. Qual foi o primeiro item do cardápio adquirido por cada cliente?
4. Qual é o item mais comprado do cardápio e quantas vezes foi comprado por todos os clientes?
5. Qual item foi o mais popular para cada cliente?
6. Qual item foi comprado primeiro pelo cliente depois que ele se tornou membro?
7. Qual item foi comprado pouco antes de o cliente se tornar um membro?
8. Qual é o total de itens e o valor gasto para cada membro antes de se tornar um membro?
9. Se cada $ 1 gasto equivale a 10 pontos e o sushi tem um multiplicador de 2x pontos - quantos pontos cada cliente teria?
10. Na primeira semana após o cliente aderir ao programa (incluindo a data de adesão), ele ganha 2x pontos em todos os itens, não apenas no sushi - quantos pontos o cliente A e B têm no final de janeiro?

# Questões Bônus

#### Juntando tudo 

As perguntas a seguir estão relacionadas à criação de tabelas de dados básicas que Danny e sua equipe podem usar para obter insights rapidamente sem precisar unir as tabelas subjacentes usando SQL.

Recrie a seguinte saída da tabela usando os dados disponíveis:

<img width="599" alt="Captura de Tela 2022-06-29 às 14 08 22" src="https://user-images.githubusercontent.com/66830501/176503938-46dcf4b6-dd1c-4acd-ab1d-3104d7a1523d.png">

#### Classificação de tudo

Danny também quer mais informações sobre a classificação dos produtos dos clientes, mas ele não precisa da classificação das compras de não membros, portanto, espera valores de classificação nulos para os registros quando os clientes ainda não fazem parte do programa de fidelidade.

<img width="695" alt="Captura de Tela 2022-06-29 às 14 16 59" src="https://user-images.githubusercontent.com/66830501/176503968-ead97642-d432-4a43-a889-09be2bf31018.png">

# Respostas

#### 1 - Qual é o valor total que cada cliente gastou no restaurante?

```sql
SELECT customer_id,
    SUM(price) AS total_amount
FROM dannys_diner.sales sales
    INNER JOIN dannys_diner.menu menu
    ON sales.product_id = menu.product_id
GROUP BY customer_id
ORDER BY total_amount DESC
``` 

**RESPOSTA:**
| customer_id  | total_amount  |
| ------------ | ------------ |
|  A | 76  |
| B  | 74  |
|  C |  36 |



#### 2 - Quantos dias cada cliente visitou o restaurante?

```sql
SELECT customer_id, 
	COUNT(order_date) AS days_visited
FROM dannys_diner.sales 
GROUP BY customer_id
ORDER BY customer_id
``` 

**RESPOSTA:**
|  customer_id | days_visited  |
| ------------ | ------------ |
| A  | 6  |
| B  |  6 |
| C  |  3 |


#### 3 - Qual foi o primeiro item do cardápio adquirido por cada cliente?

```sql
 SELECT 
	customer_id, order_date, product_name
FROM dannys_diner.sales sales
	INNER JOIN dannys_diner.menu menu
    ON sales.product_id = menu.product_id
WHERE order_date = '2021-01-01'
``` 

**RESPOSTA:**
| customer_id  | order_date  | product_name  |
| ------------ | ------------ | ------------ |
|  A | 2021-01-01  | sushi  |
|  A | 2021-01-01  |  curry |
|  B |  2021-01-01 | curry  |
|  C | 2021-01-01  | ramen  |
| C  |  2021-01-01 |  ramen |


#### 4 - Qual é o item mais comprado do cardápio e quantas vezes foi comprado por todos os clientes?

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

**RESPOSTA:**
|  product_name | favorite  |
| ------------ | ------------ |
|  ramen | 8  |
| curry  |  4 |
|  sushi |  3 |


#### 5 - Qual item foi o mais popular para cada cliente?

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

**RESPOSTA:**
|  customer_id |  product_name |  favorite |
| ------------ | ------------ | ------------ |
|  A | ramen  |  3 |
|  C | ramen  |  3 |
|  A | curry  |  2 |
|  B | curry  |  2 |
| B  |  sushi | 2  |
|  B | ramen  | 2  |
|  A | sushi  |  1 |


#### 6 - Qual item foi comprado primeiro pelo cliente depois que ele se tornou membro?

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
 
 **RESPOSTA**
 | customer_id  | product_name   |  order_date |
| ------------ | ------------ | ------------ |
|  A | curry  | 2021-01-07  |
|  A | ramen  |  2021-01-10 |
|  A |  ramen | 2021-01-11  |
|  A |  ramen |  2021-01-11 |
|  B |  sushi |  2021-01-11 |
|  B | ramen  | 2021-01-16  |
| B  |  ramen | 2021-02-01  |


####7 - Qual item foi comprado pouco antes de o cliente se tornar um membro?

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

**RESPOSTA**
| customer_id  | product_name  |  order_date |
| ------------ | ------------ | ------------ |
|  B | curry  |  2021-01-04 |
|  A |  sushi | 2021-01-02  |
|  A | curry  | 2021-01-01  |
|  B |  curry | 2021-01-01  |
| B  | curry  |  2021-01-01 |


#### 8 - Qual é o total de itens e o valor gasto para cada membro antes de se tornar um membro?

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

**RESPOSTA**
| customer_id  |  total_items |  total_amount |
| ------------ | ------------ | ------------ |
|  A | 2  |  25 |
|  B |  3 | 40  |


#### 9 - Se cada $ 1 gasto equivale a 10 pontos e o sushi tem um multiplicador de 2x pontos - quantos pontos cada cliente teria?

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

**RESPOSTA**
|  customer_id | points  |
| ------------ | ------------ |
|  A |  860 |
| B  |  940 |
|  C | 360  |


#### 10 - Na primeira semana após o cliente aderir ao programa (incluindo a data de adesão), ele ganha 2x pontos em todos os itens, não apenas no sushi - quantos pontos os clientes A e B têm no final de janeiro?

**OBS :** O CÓDIGO ABAIXO FOI FEITO NO BIGQUERY.

**OBS2 **: Considerei que os pontos passam a ser contabilizados apenas quando os cliente se torna membro do programa de fidelidade. 

```sql
WITH all_data AS
(
  SELECT sales.customer_id, order_date, sales.product_id,join_date,
  product_name, price
  FROM
   ...case_study.sales` as sales 
   INNER JOIN ...case_study.menu` as menu
   ON sales.product_id = menu.product_id
   INNER JOIN ...case_study.members` as members
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

**RESPOSTA**
| customer_id  | points |
| -------------------| ------------ |
|  A | 1020  |
|  B | 440  |



# Resolução das Questões Bônus

#### 11 - Juntando tudo 

```sql
WITH all_data AS
(
  SELECT sales.customer_id, order_date, sales.product_id,join_date,
  product_name, price,
  FROM
  ...case_study.sales` as sales 
   INNER JOIN ...case_study.menu` as menu
   ON sales.product_id = menu.product_id
   INNER JOIN ...case_study.members` as members
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
