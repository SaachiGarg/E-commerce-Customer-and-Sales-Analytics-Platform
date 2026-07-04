# Payments Analytics

## **1. Which payment methods are used most frequently?**

##### **Query:**

```sql
SELECT
    payment_type AS PaymentMethod,
    COUNT(*) AS TotalTransactions
FROM payments
GROUP BY payment_type
ORDER BY TotalTransactions DESC;
```

##### **Result:**

**Credit Card** is the most frequently used payment method, followed by **Boleto** and **Voucher**.

---

## **2. Which payment methods generate the highest revenue?**

##### **Query:**

```sql
SELECT
    payment_type AS PaymentMethod,
    ROUND(SUM(payment_value), 2) AS Revenue
FROM payments
GROUP BY payment_type
ORDER BY Revenue DESC;
```

##### **Result:**

**Credit Card** generates the highest revenue, followed by **Boleto** and **Voucher**.

---

## **3. Do installment payments correspond to larger order values?**

##### **Query:**

```sql
SELECT
    payment_installments,
    ROUND(AVG(order_value), 2) AS AverageOrderValue,
    COUNT(*) AS TotalOrders
FROM (
    SELECT
        order_id,
        MAX(payment_installments) AS payment_installments,
        SUM(payment_value) AS order_value
    FROM payments
    GROUP BY order_id
) t
GROUP BY payment_installments
ORDER BY payment_installments;
```

##### **Result:**

The analysis indicates that **orders paid in higher installments generally correspond to larger order values**, suggesting that customers tend to finance more expensive purchases through installment payments.

---

## **4. What is the average number of installments chosen?**

##### **Query:**

```sql
SELECT
    ROUND(AVG(payment_installments), 2) AS AverageInstallments
FROM (
    SELECT
        order_id,
        MAX(payment_installments) AS payment_installments
    FROM payments
    GROUP BY order_id
) t;
```

##### **Result:**

The average number of installments chosen per order is **approximately 3**.

---
