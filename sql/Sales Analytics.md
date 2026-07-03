# Sales Analytics

## **1. Which product categories generate the highest revenue?**

##### **Query:**

```sql
SELECT
    p.product_category_name AS Category,
    ROUND(SUM(oi.price)) AS Revenue
FROM order_items oi
JOIN products p
    ON oi.product_id = p.product_id
GROUP BY 1
ORDER BY Revenue DESC;
```

##### **Result:**

**Health & Beauty** is the highest revenue-generating product category.

---

## **2. How has monthly revenue changed over time?**

##### **Query:**

```sql
SELECT
    DATE_FORMAT(o.order_purchase_timestamp, '%Y-%m') AS Month,
    ROUND(SUM(oi.price + oi.freight_value), 2) AS Revenue
FROM orders o
JOIN order_items oi
    ON o.order_id = oi.order_id
GROUP BY Month
ORDER BY Month;
```

##### **Result:**

The platform experienced rapid growth during **2017–2018**.

---

## **3. Which months experience the highest order volumes?**

##### **Query:**

```sql
SELECT
    DATE_FORMAT(order_purchase_timestamp, '%Y-%m') AS Month,
    COUNT(order_id) AS TotalOrders
FROM orders
GROUP BY Month
ORDER BY TotalOrders DESC;
```

##### **Result:**

**November 2017** experienced the highest order volume, followed by **January 2018** and **March 2018**.

---

## **4. What is the average order value?**

##### **Query:**

```sql
SELECT
    ROUND(AVG(order_value)) AS AverageOrderValue
FROM (
    SELECT
        SUM(price) AS order_value
    FROM order_items
    GROUP BY order_id
) t;
```

##### **Result:**

The average order value is **138**.

---
