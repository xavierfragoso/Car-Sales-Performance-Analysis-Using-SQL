# Product Sales Analysis Using MySQL

## Tools Used
- MySQL Workbench
- CSV dataset

## Key Question & Insight

### Q: Find the product_id's of products where the total quantity sold is greater than the average total quantity sold of all products that have a total sales amount (quantity × price) greater than $10,000

```sql
SELECT product_id, SUM(quantity) AS total_quantity
FROM sales
GROUP BY product_id
HAVING SUM(quantity) > (
    SELECT AVG(total_quantity)
    FROM (
        SELECT SUM(quantity) AS total_quantity
        FROM sales
        GROUP BY product_id
        HAVING SUM(quantity * price) > 10000
    ) AS sub
);
```

> **Insight:**  
> This query helps identify products that sell a larger volume of units compared to the average volume of products generating significant revenue (over $10,000 in total sales). By focusing on products with strong sales revenue, it filters out lower-performing items that might skew averages. This highlights volume leaders that may need priority in inventory management or marketing focus.
