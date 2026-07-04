# Product Analytics

## **1. Which product categories have the highest average price?**

##### **Query:**

```sql
SELECT
    p.product_category_name AS Category,
    ROUND(AVG(oi.price), 2) AS AveragePrice
FROM products p
JOIN order_items oi
    ON p.product_id = oi.product_id
GROUP BY p.product_category_name
ORDER BY AveragePrice DESC;
```

##### **Result:**

**Computers** have the highest average selling price, followed by **Small Appliances Home** and **Agro Industry & Commerce**.

---

## **2. Which categories receive the highest ratings?**

##### **Query:**

```sql
SELECT
    p.product_category_name AS Category,
    ROUND(AVG(r.review_score), 2) AS AverageRating
FROM products p
JOIN order_items oi
    ON p.product_id = oi.product_id
JOIN reviews r
    ON oi.order_id = r.order_id
GROUP BY p.product_category_name
ORDER BY AverageRating DESC;
```

##### **Result:**

**CDs, DVDs & Musicals** received the highest average rating, followed by **Fashion Children's Clothes**.

> **Note:** Reviews in the Olist dataset are recorded at the **order level**, so the same review score is assigned to all products within an order.

---

## **3. Which categories have the highest sales volume?**

##### **Query:**

```sql
SELECT
    p.product_category_name AS Category,
    COUNT(*) AS ItemsSold
FROM products p
JOIN order_items oi
    ON p.product_id = oi.product_id
GROUP BY p.product_category_name
ORDER BY ItemsSold DESC;
```

##### **Result:**

**Bed Bath Table** has the highest sales volume, followed by **Health Beauty** and **Sports Leisure**.

---

## **4. Which categories underperform in revenue?**

##### **Query:**

```sql
SELECT
    p.product_category_name AS Category,
    ROUND(SUM(oi.price), 2) AS Revenue
FROM products p
JOIN order_items oi
    ON p.product_id = oi.product_id
GROUP BY p.product_category_name
ORDER BY Revenue ASC
LIMIT 10;
```

##### **Result:**

**Security & Services** is the lowest revenue-generating category, followed by **Fashion Children's Clothes** and **CDs, DVDs & Musicals**.

---
