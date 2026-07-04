# Reviews Analytics

## **1. What is the distribution of review scores?**

##### **Query:**

```sql
SELECT
    review_score,
    COUNT(*) AS TotalReviews,
    ROUND(
        COUNT(*) * 100.0 / (SELECT COUNT(*) FROM reviews),
        2
    ) AS Percentage
FROM reviews
GROUP BY review_score
ORDER BY review_score;
```

##### **Result:**

The distribution of review scores is as follows:

- **1 Star:** 11.51%
- **2 Stars:** 3.18%
- **3 Stars:** 8.24%
- **4 Stars:** 19.29%
- **5 Stars:** 57.78%

---

## **2. Which product categories receive the lowest ratings?**

##### **Query:**

```sql
SELECT
    p.product_category_name AS Category,
    COUNT(r.review_id) AS TotalReviews,
    ROUND(AVG(r.review_score), 2) AS AverageRating
FROM products p
JOIN order_items oi
    ON p.product_id = oi.product_id
JOIN reviews r
    ON oi.order_id = r.order_id
GROUP BY p.product_category_name
HAVING COUNT(r.review_id) >= 30
ORDER BY AverageRating ASC;
```

##### **Result:**

**Diapers & Hygiene** received the lowest average rating, followed by **Office Furniture** and **Fashion Male Clothing**.

> **Note:** Only categories with **30 or more reviews** were considered to ensure meaningful comparisons.

---

## **3. Which sellers consistently receive poor reviews?**

##### **Query:**

```sql
SELECT
    s.seller_id,
    s.seller_state,
    COUNT(r.review_id) AS TotalReviews,
    ROUND(AVG(r.review_score), 2) AS AverageRating
FROM sellers s
JOIN order_items oi
    ON s.seller_id = oi.seller_id
JOIN reviews r
    ON oi.order_id = r.order_id
GROUP BY
    s.seller_id,
    s.seller_state
HAVING COUNT(r.review_id) >= 30
ORDER BY
    AverageRating ASC,
    TotalReviews DESC
LIMIT 10;
```

##### **Result:**

Seller **1ca7077d890b907f89be8c954a02686a** consistently received the poorest customer reviews, followed by **2709af9587499e95e803a6498a5a56e9** and **bb135baca94c82fcb731335ad5b04a03**.

> **Note:** Only sellers with **30 or more reviews** were included to ensure a fair comparison.

---

## **4. How do review scores vary across states?**

##### **Query:**

```sql
SELECT
    c.customer_state AS State,
    COUNT(r.review_id) AS TotalReviews,
    ROUND(AVG(r.review_score), 2) AS AverageReviewScore
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id
JOIN reviews r
    ON o.order_id = r.order_id
GROUP BY c.customer_state
HAVING COUNT(r.review_id) >= 30
ORDER BY AverageReviewScore DESC;
```

##### **Result:**

**Amapá (AP)** recorded the highest average review score, followed by **Paraná (PR)** and **Amazonas (AM)**, while **Roraima (RR)** had the lowest average review score.

> **Note:** States with fewer than **30 reviews** were excluded from the analysis to ensure reliable comparisons.

---
