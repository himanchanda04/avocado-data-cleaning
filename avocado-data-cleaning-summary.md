## 🧼 Data Cleaning Process (Detailed)

1. **Initial Inspection**
   - Found columns with bad formatting: `Unnamed: 0`, text-based `Date`
   - Spotted possible duplicates and negative values

2. **Step-by-Step Cleaning**
   - Dropped unnecessary column `Unnamed: 0`
   - Converted `Date` column from TEXT to `DATE` using `STR_TO_DATE`
   - Added `id` as a unique identifier
   - Removed duplicate rows using `ROW_NUMBER()`
   - Replaced negative prices and volumes with NULL
   - Standardized `type` and `region` using `LOWER()` and `TRIM()`
   - Added a new column `avg_price` to show regional price averages

3. **Final Export**
   - Exported cleaned dataset as `avocado_cleaned.csv`
