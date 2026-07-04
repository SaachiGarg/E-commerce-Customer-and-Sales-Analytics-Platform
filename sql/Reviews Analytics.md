### Reviews Analytics



1.What is the distribution of review scores?



Query:



SELECT

&#x20;   review\_score,

&#x20;   COUNT(\*) AS TotalReviews,

&#x20;   ROUND(

&#x20;       COUNT(\*) \* 100.0 / (SELECT COUNT(\*) FROM reviews),

&#x20;       2

&#x20;   ) AS Percentage

FROM reviews

GROUP BY review\_score

ORDER BY review\_score;





Result:



review score 1 - 11.51%

review score 2 - 3.18%

review score 3 - 8.24%

review score 4 - 19.29%

review score 5 - 57.78%





2.Which product categories receive the lowest ratings?



Query:



SELECT

&#x20;   p.product\_category\_name AS Category,

&#x20;   COUNT(r.review\_id) AS TotalReviews,

&#x20;   ROUND(AVG(r.review\_score), 2) AS AverageRating

FROM products p

JOIN order\_items oi

&#x20;   ON p.product\_id = oi.product\_id

JOIN reviews r

&#x20;   ON oi.order\_id = r.order\_id

GROUP BY p.product\_category\_name

HAVING COUNT(r.review\_id) >= 30

ORDER BY AverageRating ASC;





Result:



diapers and hygiene receive lowest ratings followed by office furniture and fashion male clothing





3.Which sellers consistently receive poor reviews?



Query:



SELECT

&#x20;   s.seller\_id,

&#x20;   s.seller\_state,

&#x20;   COUNT(r.review\_id) AS TotalReviews,

&#x20;   ROUND(AVG(r.review\_score), 2) AS AverageRating

FROM sellers s

JOIN order\_items oi

&#x20;   ON s.seller\_id = oi.seller\_id

JOIN reviews r

&#x20;   ON oi.order\_id = r.order\_id

GROUP BY

&#x20;   s.seller\_id,

&#x20;   s.seller\_state

HAVING COUNT(r.review\_id) >= 30

ORDER BY

&#x20;   AverageRating ASC,

&#x20;   TotalReviews DESC

LIMIT 10;





Result:



1ca7077d890b907f89be8c954a02686a consistently receive poor reviews followed by 2709af9587499e95e803a6498a5a56e9 and bb135baca94c82fcb731335ad5b04a03





4.How do review scores vary across states?



Query:



SELECT

&#x20;   c.customer\_state AS State,

&#x20;   COUNT(r.review\_id) AS TotalReviews,

&#x20;   ROUND(AVG(r.review\_score), 2) AS AverageReviewScore

FROM customers c

JOIN orders o

&#x20;   ON c.customer\_id = o.customer\_id

JOIN reviews r

&#x20;   ON o.order\_id = r.order\_id

GROUP BY c.customer\_state

HAVING COUNT(r.review\_id) >= 30

ORDER BY AverageReviewScore DESC;





Result:



AP has highest review score followed by PR and AM while RR has lowest review score



