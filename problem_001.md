## Scenario
You are the lead analyst, and you have been given three unorganized tables. The CEO wants a single report and summary to answer one question:
“What is the total Revenue for each Department in Q1?”


## Pandas Solution
```python
# Pandas using jupyter
import pandas as pd
emp_data = {
    'EmployeeID': [1, 2, 3],
    'Name': ['Ali', 'Sara', 'Hassan'],
    'Department': ['Sales', 'Support', 'Sales']
}
employees_df = pd.DataFrame(emp_data)

# Pandas
import numpy as np
sales_data = {
    'TransactionID': ['T1', 'T2', 'T3', 'T4'],
    'SalespersonID': [1, 3, 1, 2], # (لـ JOIN مع EmployeeID)
    'OrderDate': ['05-01-2023', '15-02-2023', '10-04-2023', '20-01-2023'], # (نصوص DD-MM-YYYY)
    'Amount': ['$500.00', ' $300', '$1000', '$150.50'] # (نصوص فوضوية Pandas)
}
sales_df = pd.DataFrame(sales_data)

sales_df['OrderDate'] = pd.to_datetime(sales_df['OrderDate'], format='%d-%m-%Y')
sales_df['Amount'] = sales_df['Amount'].str.strip().str.replace('$', '').astype('float')
merged_df = pd.merge(employees_df, sales_df, how='inner', left_on='EmployeeID', right_on='SalespersonID')
Hello = merged_df[merged_df['OrderDate'].dt.month.isin([1,2,3])].groupby('Department')['Amount'].sum().reset_index()
Hello.rename(columns={'Amount':'TotalRevenue'})

```

## SQL Server Solution
```sql
WITH CC AS(
SELECT e.Department, CONVERT(DATE, s.OrderDate, 105) AS 'OrderDate', 
CAST(REPLACE(TRIM(s.Amount),'$', '') AS DECIMAL(10,3)) AS 'Amount'
FROM dbo.Employees e
INNER JOIN dbo.Sales s ON e.EmployeeID = s.SalespersonID
WHERE MONTH(CONVERT(DATE, s.OrderDate, 105)) IN (1,2,3)
)
SELECT Department, SUM(Amount) AS 'TotalRevenue' FROM CC
GROUP BY Department

```
