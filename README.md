# Hospital Revenue & Patient Analysis Using MySQL

## Tools Used
- MySQL Workbench
- CSV dataset

## Key Question & Insight

### Q: Which insurance providers generated the highest total billing amount in 2024, and how many unique patients did they cover?

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
