### **Sales Analytics:**



1. **Which product categories generate the highest revenue?**

   ---

##### **Query:**


SELECT

&#x20;   p.product\_category\_name\_english,

&#x20;   ROUND(SUM(oi.price),2) AS Revenue

FROM order\_items oi

JOIN products p

ON oi.product\_id = p.product\_id

GROUP BY 1

ORDER BY Revenue DESC;



##### **Result:**

##### 

Health and Beauty is the highest revenue generating product category




#### **2. How has monthly revenue changed over time?**





##### **Query:**



SELECT

&#x20;   DATE\_FORMAT(o.order\_purchase\_timestamp, '%Y-%m') AS month,

&#x20;   ROUND(SUM(oi.price + oi.freight\_value),2) AS revenue

FROM orders o

JOIN order\_items oi

ON o.order\_id = oi.order\_id

GROUP BY month

ORDER BY month;





##### **Result:**



The platform experienced rapid growth during 2017–2018.





#### **3. Which months experience the highest order volumes?**



##### **Query:**



SELECT

&#x20;   DATE\_FORMAT(order\_purchase\_timestamp, '%Y-%m') AS Month,

&#x20;   COUNT(order\_id) AS Total\_Orders

FROM orders

GROUP BY Month

ORDER BY Total\_Orders DESC;





##### **Result:**



November 2017 experienced highest order volume followed by January 2018 and March 2018




#### **4. What is the average order value?**

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




##### **Result:**



The average order value is 138



