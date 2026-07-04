### Delivery \& Logistics Analytics



1.What is the average delivery time?



Query:



SELECT

&#x20;   ROUND(

&#x20;       AVG(

&#x20;           DATEDIFF(

&#x20;               order\_delivered\_customer\_date,

&#x20;               order\_purchase\_timestamp

&#x20;           )

&#x20;       ),

&#x20;       2

&#x20;   ) AS AverageDeliveryDays

FROM orders

WHERE

&#x20;   order\_status = 'delivered'

&#x20;   AND order\_delivered\_customer\_date IS NOT NULL;





Result:



Average delivery days are 12.5





2.Which states experience the longest delivery delays?





Query:



SELECT

&#x20;   c.customer\_state AS State,

&#x20;   ROUND(

&#x20;       AVG(

&#x20;           TIMESTAMPDIFF(

&#x20;               HOUR,

&#x20;               o.order\_purchase\_timestamp,

&#x20;               o.order\_delivered\_customer\_date

&#x20;           ) / 24

&#x20;       ),

&#x20;       2

&#x20;   ) AS AverageDeliveryDays

FROM customers c

JOIN orders o

&#x20;   ON c.customer\_id = o.customer\_id

WHERE

&#x20;   o.order\_status = 'delivered'

&#x20;   AND o.order\_delivered\_customer\_date IS NOT NULL

GROUP BY c.customer\_state

ORDER BY AverageDeliveryDays DESC;





Result:



RR experience longest delivery delays followed by AP and AM





3.What percentage of orders are delivered late?



Query:



SELECT

&#x20;   ROUND(

&#x20;       100.0 \*

&#x20;       SUM(

&#x20;           CASE

&#x20;               WHEN order\_delivered\_customer\_date > order\_estimated\_delivery\_date

&#x20;               THEN 1

&#x20;               ELSE 0

&#x20;           END

&#x20;       ) / COUNT(\*),

&#x20;       2

&#x20;   ) AS LateOrderPercentage

FROM orders

WHERE

&#x20;   order\_status = 'delivered'

&#x20;   AND order\_delivered\_customer\_date IS NOT NULL;





Result:



8.11% orders are delivered late





4.Does longer delivery time lead to lower review scores?



Query:



SELECT

&#x20;   r.review\_score,

&#x20;   ROUND(

&#x20;       AVG(

&#x20;           TIMESTAMPDIFF(

&#x20;               HOUR,

&#x20;               o.order\_purchase\_timestamp,

&#x20;               o.order\_delivered\_customer\_date

&#x20;           ) / 24

&#x20;       ),

&#x20;       2

&#x20;   ) AS AverageDeliveryDays,

&#x20;   COUNT(\*) AS TotalOrders

FROM orders o

JOIN reviews r

&#x20;   ON o.order\_id = r.order\_id

WHERE

&#x20;   o.order\_status = 'delivered'

&#x20;   AND o.order\_delivered\_customer\_date IS NOT NULL

GROUP BY r.review\_score

ORDER BY r.review\_score DESC;





Result:



Yes, longer delivery time leads to lower review scores





5.Which product categories have the longest delivery times?



Query:



SELECT

&#x20;   p.product\_category\_name AS Category,

&#x20;   ROUND(

&#x20;       AVG(

&#x20;           TIMESTAMPDIFF(

&#x20;               HOUR,

&#x20;               o.order\_purchase\_timestamp,

&#x20;               o.order\_delivered\_customer\_date

&#x20;           ) / 24

&#x20;       ),

&#x20;       2

&#x20;   ) AS AverageDeliveryDays

FROM products p

JOIN order\_items oi

&#x20;   ON p.product\_id = oi.product\_id

JOIN orders o

&#x20;   ON oi.order\_id = o.order\_id

WHERE

&#x20;   o.order\_status = 'delivered'

&#x20;   AND o.order\_delivered\_customer\_date IS NOT NULL

GROUP BY p.product\_category\_name

ORDER BY AverageDeliveryDays DESC;



Result:



office furniture has longest delivery times followed by Christmas supplies and fashion shoes







