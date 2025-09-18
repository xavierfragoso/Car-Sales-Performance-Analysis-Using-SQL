# Healthcare Analytics Project

## Tools Used
- MySQL Workbench
- CSV dataset
- Tableau

## Key Questions & Insights

### Business Question 1: Which insurance providers generated the highest total billing amount in 2024, and how many unique patients did they cover?

```sql
SELECT 
    insurance_provider,
    COUNT(DISTINCT Name) AS unique_patients,
    SUM(billing_amount) AS total_billing
FROM health
WHERE YEAR(discharge_date) = 2024
GROUP BY insurance_provider
ORDER BY total_billing DESC;
```

> **Insight:**  
> This query identifies which insurance providers drove the most revenue for the hospital in 2024 while also showing how many patients they covered. Focusing on high-revenue providers helps hospital management understand which payers contribute most to overall revenue, and which patient populations are largest — useful for strategic planning, resource allocation, and negotiating contracts with insurance companies.

## Business Question 2: Which patients had a total billing amount greater than the average total billing across all patients in 2024?

### SQL Query
```sql
SELECT 
    Name,
    SUM(billing_amount) AS total_billing
FROM health
WHERE YEAR(discharge_date) = 2024
GROUP BY Name
HAVING SUM(billing_amount) > (
    SELECT AVG(total_billing)
    FROM (
        SELECT 
            SUM(billing_amount) AS total_billing
        FROM health
        WHERE YEAR(discharge_date) = 2024
        GROUP BY Name
    ) AS patient_totals
)
ORDER BY total_billing DESC;
```

> **Insight:**  
> This query identifies high-value patients whose billing exceeds the average patient billing amount in 2024. From a hospital’s perspective, these patients represent a disproportionate share of revenue and may be key for understanding utilization patterns or prioritizing care management.
