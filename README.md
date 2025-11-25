# Healthcare Analytics Project

## Tools Used
- MySQL Workbench
- CSV dataset from Kaggle
- Tableau

## Project Overview
Analyzed hospital billing data to uncover high-revenue insurance providers, identify top-paying patients, and evaluate patient contributions by doctor. Demonstrated intermediate SQL skills including aggregations, subqueries, CTEs, and window functions, and used Tableau to visualize key insights that support hospital revenue optimization and strategic care planning.

## Key Questions & Insights

### Business Question 1: Which insurance providers generated the highest total billing amount in 2024, and how many unique patients did they cover?

### SQL Query
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
> Identifies which insurance providers drove the most revenue in 2024 and how many patients they covered. This helps hospital leadership understand which payers contribute most to overall revenue and which patient populations are largest — valuable for strategic planning, resource allocation, and negotiating insurance contracts.

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
> Identifies high-value patients whose billing exceeds the average. These patients represent a disproportionate share of revenue and can help hospitals understand utilization patterns, case severity, and opportunities for targeted care management.


## Business Question 3: Which patients generated the highest billing amounts for each doctor in 2024, and who are the top 3 patients per doctor?

### SQL Query
```sql
WITH patient_totals AS (
    SELECT 
        doctor,
        name AS patient,
        SUM(billing_amount) AS total_billing
    FROM health
    WHERE YEAR(discharge_date) = 2024
    GROUP BY doctor, name
),
ranked_patients AS (
    SELECT 
        doctor,
        patient,
        total_billing,
        RANK() OVER (PARTITION BY doctor ORDER BY total_billing DESC) AS patient_rank
    FROM patient_totals
)
SELECT 
    doctor,
    patient,
    total_billing,
    patient_rank
FROM ranked_patients
WHERE patient_rank <= 3
ORDER BY doctor, patient_rank;
```

> **Insight:**  
> Reveals the top 3 highest-billing patients for each doctor. Ranking patients within each doctor’s panel highlights who contributes most to revenue, uncovering patterns related to case complexity, resource utilization, and opportunities for strategic care alignment.

## Tableau Dashboard

View the interactive dashboard here: [Healthcare Revenue & Patient Insights (2024)](https://public.tableau.com/app/profile/xavier.fragoso/viz/HospitalAnalyticsDashboard_17580762106200/HealthcareRevenuePatientInsights2024)
