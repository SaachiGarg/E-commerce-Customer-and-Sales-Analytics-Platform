# Delivery & Logistics Analytics

## **1. What is the average delivery time?**

##### **Query:**

```sql
SELECT
    ROUND(
        AVG(
            DATEDIFF(
                order_delivered_customer_date,
                order_purchase_timestamp
            )
        ),
        2
    ) AS AverageDeliveryDays
FROM orders
WHERE
    order_status = 'delivered'
    AND order_delivered_customer_date IS NOT NULL;
```

##### **Result:**

The average delivery time is **12.5 days**.

---

## **2. Which states experience the longest delivery delays?**

##### **Query:**

```sql
SELECT
    c.customer_state AS State,
    ROUND(
        AVG(
            TIMESTAMPDIFF(
                HOUR,
                o.order_purchase_timestamp,
                o.order_delivered_customer_date
            ) / 24
        ),
        2
    ) AS AverageDeliveryDays
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id
WHERE
    o.order_status = 'delivered'
    AND o.order_delivered_customer_date IS NOT NULL
GROUP BY c.customer_state
ORDER BY AverageDeliveryDays DESC;
```

##### **Result:**

**Roraima (RR)** experiences the longest delivery delays, followed by **Amapá (AP)** and **Amazonas (AM)**.

---

## **3. What percentage of orders are delivered late?**

##### **Query:**

```sql
SELECT
    ROUND(
        100.0 *
        SUM(
            CASE
                WHEN order_delivered_customer_date > order_estimated_delivery_date
                THEN 1
                ELSE 0
            END
        ) / COUNT(*),
        2
    ) AS LateOrderPercentage
FROM orders
WHERE
    order_status = 'delivered'
    AND order_delivered_customer_date IS NOT NULL;
```

##### **Result:**

**8.11%** of delivered orders were delivered later than the estimated delivery date.

---

## **4. Does longer delivery time lead to lower review scores?**

##### **Query:**

```sql
SELECT
    r.review_score,
    ROUND(
        AVG(
            TIMESTAMPDIFF(
                HOUR,
                o.order_purchase_timestamp,
                o.order_delivered_customer_date
            ) / 24
        ),
        2
    ) AS AverageDeliveryDays,
    COUNT(*) AS TotalOrders
FROM orders o
JOIN reviews r
    ON o.order_id = r.order_id
WHERE
    o.order_status = 'delivered'
    AND o.order_delivered_customer_date IS NOT NULL
GROUP BY r.review_score
ORDER BY r.review_score DESC;
```

##### **Result:**

The analysis indicates that **longer delivery times are associated with lower customer review scores**, suggesting that delivery performance has a significant impact on customer satisfaction.

---

## **5. Which product categories have the longest delivery times?**

##### **Query:**

```sql
SELECT
    p.product_category_name AS Category,
    ROUND(
        AVG(
            TIMESTAMPDIFF(
                HOUR,
                o.order_purchase_timestamp,
                o.order_delivered_customer_date
            ) / 24
        ),
        2
    ) AS AverageDeliveryDays
FROM products p
JOIN order_items oi
    ON p.product_id = oi.product_id
JOIN orders o
    ON oi.order_id = o.order_id
WHERE
    o.order_status = 'delivered'
    AND o.order_delivered_customer_date IS NOT NULL
GROUP BY p.product_category_name
ORDER BY AverageDeliveryDays DESC;
```

##### **Result:**

**Office Furniture** has the longest average delivery time, followed by **Christmas Supplies** and **Fashion Shoes**.

---
