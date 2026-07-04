### Sellers Analytics



1.Which sellers generate the highest revenue?



Query:



SELECT

&#x20;   oi.seller\_id,

&#x20;   ROUND(SUM(oi.price), 2) AS Revenue

FROM order\_items oi

GROUP BY oi.seller\_id

ORDER BY Revenue DESC

LIMIT 10;





Result:



4869f7a5dfa277a7dca6462dcf3b52b2 generated highest revenue followed by 53243585a1d6dc2643021fd1853d8905 and 4a3ca9315b744ce9f8e9374361493884





2.Which sellers receive the highest customer ratings?



Query:



SELECT

&#x20;   oi.seller\_id,

&#x20;   ROUND(AVG(r.review\_score), 2) AS AverageRating

FROM order\_items oi

JOIN reviews r

&#x20;   ON oi.order\_id = r.order\_id

GROUP BY oi.seller\_id

ORDER BY AverageRating DESC;





Result:



5f57db27027655e6c6a391601daa0258 has the highest customer ratings followed by 72431a818f97fe6ab9c81eee3e297e54 and dd264199fc8b687ad029de7de6d760e6





3.Which states contain the most sellers?





Query:



SELECT

&#x20;   seller\_state AS State,

&#x20;   COUNT(seller\_id) AS TotalSellers

FROM sellers

GROUP BY seller\_state

ORDER BY TotalSellers DESC;





Result:



SP contain most sellers followed by PR and MG





4.Which sellers have the fastest delivery times?



Query:



SELECT

&#x20;   oi.seller\_id,

&#x20;   ROUND(

&#x20;       AVG(

&#x20;           DATEDIFF(

&#x20;               o.order\_delivered\_customer\_date,

&#x20;               o.order\_purchase\_timestamp

&#x20;           )

&#x20;       ),

&#x20;       2

&#x20;   ) AS AverageDeliveryDays

FROM order\_items oi

JOIN orders o

&#x20;   ON oi.order\_id = o.order\_id

WHERE

&#x20;   o.order\_delivered\_customer\_date IS NOT NULL

GROUP BY oi.seller\_id

ORDER BY AverageDeliveryDays ASC

LIMIT 10;





Result:



6561d6bf844e464b4019442692b40e02 has fastest delivery time followed by 139157dd4daa45c25b0807ffff348363 and e063e85d44b0f5c3e6ec3131103a57e


5.Which sellers have the lowest review scores?



Query:



SELECT

&#x20;   oi.seller\_id,

&#x20;   COUNT(r.review\_id) AS TotalReviews,

&#x20;   ROUND(AVG(r.review\_score), 2) AS AverageRating

FROM order\_items oi

JOIN reviews r

&#x20;   ON oi.order\_id = r.order\_id

GROUP BY oi.seller\_id

HAVING COUNT(r.review\_id) >= 30

ORDER BY AverageRating ASC

LIMIT 10;





Result:



1ca7077d890b907f89be8c954a02686a has lowest review score followed by 2709af9587499e95e803a6498a5a56e9 and bb135baca94c82fcb731335ad5b04a03 









