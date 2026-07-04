# Sellers Analytics

## **1. Which sellers generate the highest revenue?**

##### **Query:**

```sql
SELECT
    oi.seller_id,
    ROUND(SUM(oi.price), 2) AS Revenue
FROM order_items oi
GROUP BY oi.seller_id
ORDER BY Revenue DESC
LIMIT 10;
```

##### **Result:**

Seller **4869f7a5dfa277a7dca6462dcf3b52b2** generated the highest revenue, followed by **53243585a1d6dc2643021fd1853d8905** and **4a3ca9315b744ce9f8e9374361493884**.

---

## **2. Which sellers receive the highest customer ratings?**

##### **Query:**

```sql
SELECT
    oi.seller_id,
    ROUND(AVG(r.review_score), 2) AS AverageRating
FROM order_items oi
JOIN reviews r
    ON oi.order_id = r.order_id
GROUP BY oi.seller_id
ORDER BY AverageRating DESC;
```

##### **Result:**

Seller **5f57db27027655e6c6a391601daa0258** received the highest average customer rating, followed by **72431a818f97fe6ab9c81eee3e297e54** and **dd264199fc8b687ad029de7de6d760e6**.

> **Note:** Reviews in the Olist dataset are recorded at the **order level**, so the same review score is assigned to every seller involved in that order.

---

## **3. Which states contain the most sellers?**

##### **Query:**

```sql
SELECT
    seller_state AS State,
    COUNT(seller_id) AS TotalSellers
FROM sellers
GROUP BY seller_state
ORDER BY TotalSellers DESC;
```

##### **Result:**

**São Paulo (SP)** contains the largest number of sellers, followed by **Paraná (PR)** and **Minas Gerais (MG)**.

---

## **4. Which sellers have the fastest delivery times?**

##### **Query:**

```sql
SELECT
    oi.seller_id,
    ROUND(
        AVG(
            DATEDIFF(
                o.order_delivered_customer_date,
                o.order_purchase_timestamp
            )
        ),
        2
    ) AS AverageDeliveryDays
FROM order_items oi
JOIN orders o
    ON oi.order_id = o.order_id
WHERE
    o.order_delivered_customer_date IS NOT NULL
GROUP BY oi.seller_id
ORDER BY AverageDeliveryDays ASC
LIMIT 10;
```

##### **Result:**

Seller **6561d6bf844e464b4019442692b40e02** has the fastest average delivery time, followed by **139157dd4daa45c25b0807ffff348363** and **e063e85d44b0f5c3e6ec3131103a57e**.

---

## **5. Which sellers have the lowest review scores?**

##### **Query:**

```sql
SELECT
    oi.seller_id,
    COUNT(r.review_id) AS TotalReviews,
    ROUND(AVG(r.review_score), 2) AS AverageRating
FROM order_items oi
JOIN reviews r
    ON oi.order_id = r.order_id
GROUP BY oi.seller_id
HAVING COUNT(r.review_id) >= 30
ORDER BY AverageRating ASC
LIMIT 10;
```

##### **Result:**

Seller **1ca7077d890b907f89be8c954a02686a** has the lowest average review score, followed by **2709af9587499e95e803a6498a5a56e9** and **bb135baca94c82fcb731335ad5b04a03**.

> **Note:** Only sellers with **30 or more reviews** are included to ensure a fair comparison and avoid misleading results from sellers with very few reviews.

---
