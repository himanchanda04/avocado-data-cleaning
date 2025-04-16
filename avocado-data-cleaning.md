# **Avocado Data Cleaning**

This is a data cleaning project using SQL on the avocado dataset.

```sql
-- View existing date values
SELECT date
FROM Avacado_dataset.avocado;

-- Add a new column for proper date format
ALTER TABLE avocado
ADD COLUMN Date_new DATE;

-- Disable SQL safe updates
SET SQL_SAFE_UPDATES = 0;

-- Convert string Date to actual Date format
UPDATE avocado
SET Date_new = STR_TO_DATE(Date, '%y-%m-%d');

-- Drop the old Date column and rename the new one
ALTER TABLE avocado
DROP COLUMN Date;
ALTER TABLE avocado CHANGE COLUMN Date_new Date DATE;

-- View distinct dates
SELECT DISTINCT date
FROM Avacado_dataset.avocado;

-- Re-update using another date format
UPDATE avocado
SET Date_new = STR_TO_DATE(Date, '%Y-%m-%d')
WHERE STR_TO_DATE(Date, '%Y-%m-%d') IS NOT NULL;

-- Trim spaces in date column
UPDATE avocado 
SET Date = TRIM(Date);

-- View date columns
SELECT Date, Date_new 
FROM Avacado_dataset.avocado;

-- View all data
SELECT *
FROM Avacado_dataset.avocado;

-- Removing an unnecessary column
ALTER TABLE avocado
DROP COLUMN `MyUnknownColumn`;

-- Check table after column removal
SELECT *
FROM Avacado_dataset.avocado;

-- Remove duplicates using row_number()
ALTER TABLE avocado 
ADD COLUMN id INT AUTO_INCREMENT PRIMARY KEY;

WITH CTE AS (
  SELECT *, 
         ROW_NUMBER() OVER (PARTITION BY region, year, type ORDER BY id) AS rn_1
  FROM Avacado_dataset.avocado
)
DELETE FROM avocado
WHERE id IN (
  SELECT id
  FROM CTE
  WHERE rn_1 > 1
);

-- Confirm IDs
SELECT id
FROM Avacado_dataset.avocado;

-- View cleaned data
SELECT *
FROM Avacado_dataset.avocado;

-- Check for negative or unusual values in numeric columns
SELECT *
FROM Avacado_dataset.avocado
WHERE AveragePrice < 0
   OR `Total Volume` < 1
   OR `4046` < 0
   OR `4770` < 0
   OR `Small Bags` < 0
   OR `Large Bags` < 0
   OR `XLarge Bags` < 0;

-- Check and trim spacing in text columns
SELECT type, TRIM(type) AS New_type,
       TRIM(region) AS new_region
FROM Avacado_dataset.avocado;

-- Apply trim and lowercase updates
UPDATE avocado
SET type = TRIM(type);

UPDATE avocado
SET region = TRIM(region);

UPDATE avocado
SET type = LOWER(type);

-- Verify total bags calculation
SELECT *
FROM Avacado_dataset.avocado
WHERE `Total Bags` = `Small Bags` + `Large Bags` + `XLarge Bags`;

-- Find regions with highest average avocado prices
SELECT region, 
       ROUND(AVG(AveragePrice), 2) AS avg_price
FROM Avacado_dataset.avocado
GROUP BY region
ORDER BY avg_price DESC;

-- Add avg_price column
ALTER TABLE avocado
ADD COLUMN avg_price DECIMAL(5,2);

-- Update avg_price using subquery
UPDATE avocado a
JOIN (
  SELECT region, 
         ROUND(AVG(AveragePrice), 2) AS avg_price
  FROM Avacado_dataset.avocado
  GROUP BY region
) AS avg_table
ON a.region = avg_table.region
SET a.avg_price = avg_table.avg_price;

-- Final dataset view
SELECT *
FROM Avacado_dataset.avocado;
```
