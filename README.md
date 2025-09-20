#  Creating a `DIM_DATE` Table in SQL

This guide explains how to **create and populate a `DIM_DATE` table** in SQL for handling **calendar dates and fiscal years** (aligned with AtliQ’s fiscal year starting in September).

---

## 🔹 Step 1: Create the Table

1. Go to **Schemas → Tables → Create Table**.

2. Name the table: **`dim_date`**.

3. Add the following columns:

   * `calendar_date` → **DATE** format
   * `fiscal_year` → **YEAR** format

4. Make `fiscal_year` **generative** by ticking `G` in the editor.

5. Use this code to auto-generate the fiscal year:

   ```sql
   YEAR((`calendar_date` + INTERVAL 4 MONTH))
   ```

   ✅ Why 4 months? AtliQ’s fiscal year starts in **September**. By adding 4 months, September becomes January, aligning with fiscal calculations.

---

## 🔹 Step 2: Create a Date Seed Table in Excel

1. Open Excel and create two columns:

   * `date`
   * `fiscal_year` (leave blank, SQL will generate it automatically).

2. Find the **first and last dates** in your transaction data (`fact_sales_monthly`):

   ```sql
   SELECT MAX(date) AS Max_date, MIN(date) AS Min_date  
   FROM fact_sales_monthly;
   ```

3. Start filling the `date` column in **monthly intervals**:

   * Example:

     ```
     2017-09-01  
     2017-10-01  
     2017-11-01  
     ...  
     ```
   * Drag down to auto-fill until the last date.

4. Save the file as **CSV** in a safe folder.

---

## 🔹 Step 3: Import Data into SQL

1. In SQL, open the `dim_date` table.

2. Click **Import**.

3. Select **Location → Use existing table → dim\_date**.

4. Map columns:

   * `date` → `calendar_date`
   * Leave `fiscal_year` **unchecked** (it’s auto-generated).

5. Click **Next → Next → Import**.

6. Done! 🎉 Your `dim_date` table is now live with **auto-generated fiscal years**.

---

## ✅ Final Notes

* `calendar_date` stores each monthly start date.
* `fiscal_year` is auto-calculated in SQL using the 4-month shift logic.
* This setup ensures **consistent fiscal year/quarter analysis** across all fact tables.

---

Would you like me to also add **quarter logic** (like your `QUARTERS` function) into this README, so that your `dim_date` can handle both **fiscal year + quarter** at once?
