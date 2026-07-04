### Customer Analytics



1.Which states contribute the highest revenue?



Query:



SELECT

&#x20;   c.customer\_state AS State,

&#x20;   ROUND(SUM(oi.price + oi.freight\_value), 2) AS Revenue

FROM customers c

JOIN orders o

&#x20;   ON c.customer\_id = o.customer\_id

JOIN order\_items oi

&#x20;   ON o.order\_id = oi.order\_id

GROUP BY State

ORDER BY Revenue DESC;





Result:



SP contributes highest revenue followed by RJ and MG





2.What percentage of customers are repeat buyers?



Query:



SELECT

&#x20;   COUNT(CASE WHEN total\_orders > 1 THEN 1 END) AS RepeatCustomers,

&#x20;   COUNT(\*) AS TotalCustomers,

&#x20;   ROUND(

&#x20;       COUNT(CASE WHEN total\_orders > 1 THEN 1 END) \* 100.0 / COUNT(\*),

&#x20;       2

&#x20;   ) AS RepeatBuyerPercentage

FROM (

&#x20;   SELECT

&#x20;       c.customer\_unique\_id,

&#x20;       COUNT(o.order\_id) AS total\_orders

&#x20;   FROM customers c

&#x20;   JOIN orders o

&#x20;       ON c.customer\_id = o.customer\_id

&#x20;   GROUP BY c.customer\_unique\_id

) t;





Result:



3.12% customers are repeat buyers





3.Which cities have the largest customer base?



Query:



SELECT

&#x20;   customer\_city AS City,

&#x20;   COUNT(DISTINCT customer\_unique\_id) AS TotalCustomers

FROM customers

GROUP BY customer\_city

ORDER BY TotalCustomers DESC;





Result:



sao Paulo has the largest customer base followed by rio de Janeiro and belo horizonte





4.What is the average number of orders per customer?



Query:



SELECT

&#x20;   ROUND(

&#x20;       COUNT(o.order\_id) / COUNT(DISTINCT c.customer\_unique\_id),

&#x20;       2

&#x20;   ) AS AverageOrdersPerCustomer

FROM customers c

JOIN orders o

&#x20;   ON c.customer\_id = o.customer\_id;





Result:



Average order per customer is 1







&#x20;

