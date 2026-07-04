# Customer Analytics

## **1. Which states contribute the highest revenue?**

##### **Query:**

```sql
SELECT
    c.customer_state AS State,
    ROUND(SUM(oi.price + oi.freight_value), 2) AS Revenue
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id
JOIN order_items oi
    ON o.order_id = oi.order_id
GROUP BY State
ORDER BY Revenue DESC;
```

##### **Result:**

**São Paulo (SP)** contributes the highest revenue, followed by **Rio de Janeiro (RJ)** and **Minas Gerais (MG)**.

---

## **2. What percentage of customers are repeat buyers?**

##### **Query:**

```sql
SELECT
    COUNT(CASE WHEN total_orders > 1 THEN 1 END) AS RepeatCustomers,
    COUNT(*) AS TotalCustomers,
    ROUND(
        COUNT(CASE WHEN total_orders > 1 THEN 1 END) * 100.0 / COUNT(*),
        2
    ) AS RepeatBuyerPercentage
FROM (
    SELECT
        c.customer_unique_id,
        COUNT(o.order_id) AS total_orders
    FROM customers c
    JOIN orders o
        ON c.customer_id = o.customer_id
    GROUP BY c.customer_unique_id
) t;
```

##### **Result:**

**3.12%** of customers are repeat buyers.

---

## **3. Which cities have the largest customer base?**

##### **Query:**

```sql
SELECT
    customer_city AS City,
    COUNT(DISTINCT customer_unique_id) AS TotalCustomers
FROM customers
GROUP BY customer_city
ORDER BY TotalCustomers DESC;
```

##### **Result:**

**São Paulo** has the largest customer base, followed by **Rio de Janeiro** and **Belo Horizonte**.

---

## **4. What is the average number of orders per customer?**

##### **Query:**

```sql
SELECT
    ROUND(
        COUNT(o.order_id) / COUNT(DISTINCT c.customer_unique_id),
        2
    ) AS AverageOrdersPerCustomer
FROM customers c
JOIN orders o
    ON c.customer_id = o.customer_id;
```

##### **Result:**

The average number of orders per customer is **1**

---
