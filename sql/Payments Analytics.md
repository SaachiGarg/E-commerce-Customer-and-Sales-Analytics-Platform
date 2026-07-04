### Payments Analytics





1.Which payment methods are used most frequently?



Query:



SELECT

&#x20;   payment\_type AS PaymentMethod,

&#x20;   COUNT(\*) AS TotalTransactions

FROM payments

GROUP BY payment\_type

ORDER BY TotalTransactions DESC;





Result:



Credit card is the most frequently used payment method followed by boleto and voucher





2.Which payment methods generate the highest revenue?





Query:



SELECT

&#x20;   payment\_type AS PaymentMethod,

&#x20;   ROUND(SUM(payment\_value), 2) AS Revenue

FROM payments

GROUP BY payment\_type

ORDER BY Revenue DESC;





Result:



Credit card generate highest revenue followed by boleto and voucher





3.Do installment payments correspond to larger order values?



Query:



SELECT

&#x20;   payment\_installments,

&#x20;   ROUND(AVG(order\_value), 2) AS AverageOrderValue,

&#x20;   COUNT(\*) AS TotalOrders

FROM (

&#x20;   SELECT

&#x20;       order\_id,

&#x20;       MAX(payment\_installments) AS payment\_installments,

&#x20;       SUM(payment\_value) AS order\_value

&#x20;   FROM payments

&#x20;   GROUP BY order\_id

) t

GROUP BY payment\_installments

ORDER BY payment\_installments;





Result:



Many cases show that installment payments correspond to larger order values





4.What is the average number of installments chosen?



SELECT

&#x20;   ROUND(AVG(payment\_installments), 2) AS AverageInstallments

FROM (

&#x20;   SELECT

&#x20;       order\_id,

&#x20;       MAX(payment\_installments) AS payment\_installments

&#x20;   FROM payments

&#x20;   GROUP BY order\_id

) t;





Result:



Average installments are approximately 3



