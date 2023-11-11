# SteelData_SteveCar_Analysis_SQL_Challenge-1


### Introduction
Steve runs a top-end car showroom but his data analyst has just quit and left him without his crucial insights.
Can you analyse the following data to provide him with all the answers he requires?

Sharing the link to [SteelData-SQL Challenge 1](https://www.steeldata.org.uk/sql1.html)

-----------------------------------------------------------------------------------------------------------------------------------------

## Table Details

| Table Name | Column Name |
| ---------- | ----------- |
| sales | Sale_id,car_id,salesman_id,purchase_date |
| cars | Car_id,make,type,style,cost_$ |
| salespersons | Salesman_id,name,age,city |

--------------------------------------------------------------------------------------------------------------------------------------

## Code

``` js
/*Create a function to get the YEAR value for any given date and can be used by calling the function to get the YEAR*/

/*  create function get_year (
   calendar_date date
)
Returns int
    DETERMINISTIC
BEGIN
DECLARE Year int;
SET Year = Year(calendar_date);
RETURN Year;
END
*/
```
--------------------------------------------------------------------------------------------------------------------------------------

*1. What are the details of all cars purchased in the year 2022?*

``` js
select *
from cars c
join sales s
using(c_id)
where year(s.purchase_date) = '2022';
``` 
 
```
                                 Output
Fetches all the cal details purchased in the year 2022

                              Concepts learned
1.YEAR()
2.USING()
3.JOIN
```
-----------------------------------------------------------------------------------------------------

*2. What is the total number of cars sold by each salesperson?*

``` js
select s.sm_id,count(s.c_id) as total_cars_sold
from sales s 
join salespersons sp 
using(sm_id)
group by s.sm_id;
``` 

```
                                 Output
Retrieve the sales data, specifically the number of units sold by each salesperson,
and identify the salesperson with the highest sales volume.
Salesperson ID 3 has achieved the highest performance, having sold a total of 6 cars.

                              Concepts learned
1.JOIN
2.USING()
3.GROUP BY

```
-----------------------------------------------------------------------------------------------------

*3. What is the total revenue generated by each salesperson?*

``` js
select sp.sm_id,sp.name,
		sum(c.cost_$) as Total_revenue
from sales s 
join salespersons sp 
using(sm_id)
join cars c
using(c_id)
group by s.sm_id,sp.name;

/*STORED PROCEDURE to get a report on the revenue for any sales person*/
/* create procedure revenue_from_saleasperson (
         IN in_salespersonID int 
)
BEGIN
select sp.sm_id,sp.name,
		sum(c.cost_$) as Total_revenue
from sales s 
join salespersons sp 
using(sm_id)
join cars c
using(c_id)
where sp.sm_id=in_salespersonID
group by s.sm_id,sp.name;
END
*/
``` 

```
                                 Output
Retrieve the sales data to ascertain the total revenue generated by individual salespersons.
Tom Lee stands out as the top performer, contributing the highest revenue among all salespeople, amounting to $253,000.
Additionally, Lucy has excelled in generating lease revenue, accumulating a noteworthy $171,000.

                              Concepts learned
1.SUM()
2.USING()
3.GROUP BY
4.STORED PROCEDURE : To get a report on the revenue generated by any salesperson

```
-----------------------------------------------------------------------------------------------------


*4. What are the details of the cars sold by each salesperson?*

``` js
select sp.sm_id,sp.name,
		c.c_id,c.make,c.type,c.style,c.cost_$,s.purchase_date
from sales s 
join salespersons sp 
using(sm_id)
join cars c
using(c_id);

/*STORED PROCEDURE : AUTOMATED to get the details of car sold for any salesperson for any sales year*/

/*CREATE DEFINER=`root`@`localhost` PROCEDURE `car_details_sold_by_each_salesperson`(
				IN in_salespersonID int,
        IN in_year year
)
BEGIN
select	c.c_id,c.make,c.type,c.style,c.cost_$,s.purchase_date
from sales s 
join salespersons sp 
using(sm_id)
join cars c
using(c_id)
where sp.sm_id=in_salespersonID
AND
get_year(s.purchase_date)=in_year;
END
*/

``` 

```
                                 Output
Fetched the details of cars sold by eash salesperson.

                              Concepts learned
1.JOIN
2.USING()
3.GROUP BY
4.STORED PROCEDURE : To get a report on car sold for any salesperson for the any sales year.

```
-----------------------------------------------------------------------------------------------------

*5. What is the total revenue generated by each car type?*

``` js
select c.type,sum(c.cost_$) as Revenue
from cars c 
join sales s 
using(c_id)
group by c.type,c.cost_$;
``` 

```
                                 Output
Retrieve data to analyze the total revenue generated by each car type.
The X5 car type emerges as the highest revenue generator, contributing $220,000.
Conversely, the A4 car type records the least revenue, amounting to $48,000.

                              Concepts learned
1.JOIN
2.USING()
3.GROUP BY

```
-----------------------------------------------------------------------------------------------------

*6. What are the details of the cars sold in the year 2021 by salesperson 'Emily Wong'?*

``` js
select sp.name,
		c.c_id,c.make,c.type,c.style,c.cost_$,s.purchase_date
from sales s 
join salespersons sp 
using(sm_id)
join cars c
using(c_id)
where sp.sm_id=2 AND year(s.purchase_date)='2021';
``` 

```
                                 Output
Retrieve analytical data encompassing the details of cars sold by the salesperson Emily during the calendar year of 2021.

                              Concepts learned
1.JOIN
2.USING()

```
-----------------------------------------------------------------------------------------------------

*7. What is the total revenue generated by the sales of hatchback cars?*

``` js
select c.style,sum(c.cost_$) as Total_Revenue
from cars c 
join sales s 
using(c_id)
where c.style='Hatchback'
;
``` 

```
                                 Output
Retrieve analytical data indicating that the cumulative revenue generated from the sales of hatchback cars amounts to $100,000.

                              Concepts learned
1.JOIN
2.USING()

```
-----------------------------------------------------------------------------------------------------

*8. What is the total revenue generated by the sales of SUV cars in the year 2022?*

``` js
select sum(cost_$) as SUV_cars_Total_Revenue
from cars c 
join sales s 
using(c_id)
where c.style = 'SUV' AND year(s.purchase_date)='2022';

/*STORED PROCEDURE for any style of car for any given year*/
/* 
create procedure revenue_any_car_type_year(
         IN in_car_type char,
         IN in_year year
)
BEGIN
    select sum(cost_$) as Total_Revenue,s.purchase_date
	from cars c 
	join sales s 
	using(c_id)
	where c.style = in_car_type 
    AND get_year(s.purchase_date)=in_year;
    END    
*/

``` 

```
                                 Output
Retrieve analytical data showcasing that the overall revenue derived from the sales of SUV car types is $150,000.

                              Concepts learned
1.JOIN
2.USING()
3.STORED PROCEDURE to get the report on revenue for any car style and given year

```
-----------------------------------------------------------------------------------------------------

*9. What is the name and city of the salesperson who sold the most number of cars in the year 2023?*

``` js
WITH cte1 AS (
    SELECT sp.sm_id, sp.name AS SalesPerson, sp.city, COUNT(*) AS cars_sold
    FROM salespersons sp
    JOIN sales s ON sp.sm_id = s.sm_id
    WHERE YEAR(s.purchase_date) = 2023
    GROUP BY sp.sm_id, sp.name, sp.city
)
 
SELECT *
FROM cte1
order by cars_sold desc
limit 1;

/*Create STORED PROCEDURE to get a report of salesperson who sold the most number of cars in any given YEAR
input - YEAR*/

/*CREATE PROCEDURE `report_salesperson_sold_most_cars_given_year` (
				IN in_year year
)
BEGIN
WITH cte1 AS (
    SELECT sp.sm_id, sp.name AS SalesPerson, sp.city, COUNT(*) AS cars_sold
    FROM salespersons sp
    JOIN sales s ON sp.sm_id = s.sm_id
    WHERE YEAR(s.purchase_date) = 2023
    GROUP BY sp.sm_id, sp.name, sp.city
)
 
SELECT *
FROM cte1
order by cars_sold desc
limit 1;
END
*/
``` 

```
                                 Output
In the analytics, it is observed that the salesperson Tom Lee, based in Seattle, has achieved the highest sales volume in the year 2023, with a total of 2 cars sold.

                              Concepts learned
1.JOIN
2.USING()
3.STORED PROCEDURE to get a report of salesperson who sold the most number of cars in any given YEAR
input - YEAR
```
-----------------------------------------------------------------------------------------------------

*10. What is the name and age of the salesperson who generated the highest revenue in the year 2022?*

``` js
with cte1 AS(
select sp.sm_id,sp.name,sp.age,sum(c.cost_$) as revenue
from salespersons sp 
join sales s 
using(sm_id)
join cars c 
using(c_id)
WHERE YEAR(s.purchase_date) = 2022
group by sp.sm_id,sp.name,sp.age
)

select *
from cte1
order by revenue desc
limit 1;

/*STORED PROCEDURE*/
/*
CREATE DEFINER=`root`@`localhost` PROCEDURE `report_salesperson_earns_highest_revenue`(
    IN in_year year
)
BEGIN
with cte1 AS(
select sp.sm_id,sp.name,sp.age,sum(c.cost_$) as revenue
from salespersons sp 
join sales s 
using(sm_id)
join cars c 
using(c_id)
WHERE YEAR(s.purchase_date) = in_year
group by sp.sm_id,sp.name,sp.age
)

select *
from cte1
order by revenue desc
limit 1;
*/  

``` 

```
                                 Output

In the analytical assessment, it is determined that Emily Wong attained the highest revenue in the year 2022, amounting to $116,000.

                              Concepts learned
1.JOIN
2.USING()
3.STORED PROCEDURE to get report on any given YEAR which slaes person earns the highest revenue

```
-----------------------------------------------------------------------------------------------------
