# Healthcare Analytics Project

## Tools Used
- MySQL Workbench
- CSV dataset
- Tableau

## Project Overview
Analyzed hospital billing data to identify high-revenue insurance providers, top-paying patients, and patient contributions by doctor. The project uses SQL and Tableau to provide actionable insights for revenue management and strategic care planning.

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
> This query highlights the top 3 highest-billing patients for each doctor in 2024. By ranking patients within each doctor’s panel, hospitals can identify which individuals contribute most to revenue under specific physicians, uncovering patterns in patient case severity, resource utilization, and opportunities for targeted care management.

## Tableau Dashboard

View the interactive dashboard here: [Healthcare Revenue & Patient Insights (2024)](https://public.tableau.com/app/profile/xavier.fragoso/viz/HospitalAnalyticsDashboard_17580762106200/HealthcareRevenuePatientInsights2024)
