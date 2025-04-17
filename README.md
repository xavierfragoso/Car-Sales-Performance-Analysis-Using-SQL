# Car Sales Performance Analysis Using SQL

## Project Overview
This project explores car sales data using SQL to analyze relationships between horsepower, price, and vehicle type. The data was imported into MySQL Workbench from a CSV file.

## Tools Used
- MySQL Workbench
- SQL
- CSV dataset

## Key Question & Insight

### Q: What is the average horsepower and price for each vehicle type?

```sql
SELECT Vehicle_type, 
       AVG(Horsepower) AS Avg_Horsepower, 
       AVG(Price_in_thousands) AS Avg_Price
FROM cars
GROUP BY Vehicle_type;
```

> **Insight:**  
> Passenger vehicles have higher average horsepower (183.29) and price ($26.29k) compared to cars (176.62 HP, $24.10k), suggesting that higher horsepower vehicles tend to be priced higher.

### Q: Which vehicle model has the highest horsepower-to-price ratio (HP per thousand)? What’s its vehicle type?

```sql
SELECT Model, 
       Vehicle_type, 
       Horsepower / Price_in_thousands AS hp_per_dollar
FROM cars
ORDER BY hp_per_dollar DESC
LIMIT 1;
```

> **Insight:**  
> The Toyota Tacoma offers the highest horsepower-to-price ratio at 12.32 HP per thousand, making it the best value for power in this dataset. It's a Car type, suggesting that for every $1,000 spent, you get 12.32 horsepower.
