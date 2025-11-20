## Scenario
The management wants to reward the top 2 sales employees (highest in Amount) in each department separately.


## Pandas Solution
```python
# Pandas using jupyter
import pandas as pd
emp_data = {
    'EmpID': [1, 2, 3, 4, 5],
    'Name': ['Ali', 'Sara', 'Hassan', 'Fatima', 'Zainab'],
    'Department': ['Sales', 'Sales', 'Support', 'Support', 'Sales']
}
employees_df = pd.DataFrame(emp_data)


sales_data = {
    'SaleID': ['S1', 'S2', 'S3', 'S4', 'S5', 'S6', 'S7'],
    'EmpID_Ref': [1, 1, 2, 3, 3, 4, 5] 
    'Amount': ['$1,200', ' $500', ' $1,300.50', '900', '$1,000', ' 400.00', '$1,100'] 
}
sales_df = pd.DataFrame(sales_data)
sales_df['Amount'] = sales_df['Amount'].str.strip().str.replace('$', '').str.replace(',', '').astype(float)

merged_df = pd.merge(employees_df, sales_df, how='inner', left_on='EmpID', right_on='EmpID_Ref')
merged_df['Total'] = merged_df.groupby('Name')['Amount'].transform('sum')
merged_df['Ranking'] = merged_df.groupby('Department')['Total'].rank(
    method='dense', ascending=False
).astype(int)
merged_df[merged_df['Ranking'].isin([1,2])][['Name', 'Department', 'Total', 'Ranking']].drop_duplicates()

```

## SQL Server Solution
```sql
WITH cte AS(
SELECT e.Name, e.Department, 
CAST(REPLACE(REPLACE(TRIM(s.Amount), '$', ''), ',', '') AS DECIMAL(10,3)) AS 'Amount'
FROM dbo.Employees e
INNER JOIN dbo.Sales s ON e.EmpID = s.EmpID_ref),
cte1 AS (
SELECT Name, Department, SUM(Amount) Total FROM cte
GROUP BY Name, Department
),
cte2 AS (
SELECT *, RANK() OVER(PARTITION BY Department ORDER BY Total DESC) AS 'Ranking' FROM cte1
)
SELECT * FROM cte2
WHERE Ranking IN (1,2)
```
