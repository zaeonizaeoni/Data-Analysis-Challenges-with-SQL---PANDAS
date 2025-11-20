## Scenario 
**You have two tables:
1.	Employees: a list of employees and the department (Dept) they work in.
2.	Sales: a record of all sales and who made them (EmployeeID).
The task: The Sales Manager wants a report showing the Top 3 Sales by Amount within each department

## Pandas Solution
```python
# Pandas using jupyter
import pandas as pd

data_emp = {
    'EmployeeID': [101, 102, 103, 104, 105, 106],
    'EmpName': ['Alice', 'Bob', 'Charlie', 'David', 'Eve', 'Frank'],
    'Dept': ['Sales', 'Sales', 'Sales', 'Support', 'Support', 'Engineering']
}
df_emp = pd.DataFrame(data_emp)

data_sales = {
    'SaleID': [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12],
    'EmployeeID': [101, 101, 102, 102, 103, 103, 104, 104, 104, 105, 105, 106],
    'Amount': [500, 550, 600, 650, 400, 450, 100, 150, 200, 300, 350, 800]
}
df_sales = pd.DataFrame(data_sales)
merged_df = pd.merge(df_emp, df_sales, on='EmployeeID', how='inner')
# merged_df = merged_df.sort_values(by=['EmployeeID'])
merged_df['Rank'] = merged_df.groupby('Dept')['Amount'].rank(method='dense', ascending=False)
merged_df[merged_df['Rank'] < 4][['Dept', 'EmpName', 'Amount', 'Rank']].sort_values(by=['Amount', 'Rank'], ascending=False).reset_index(drop=True)

```

## SQL Server Solution
```sql
WITH Ranking_CTE AS (
SELECT 
    e.Dept, 
    e.EmpName, 
    s.Amount, 
    RANK() OVER(PARTITION BY e.Dept ORDER BY s.Amount DESC) AS 'Rank'
FROM dbo.Employees e
INNER JOIN dbo.Sales s ON e.EmployeeID = s.EmployeeID)
SELECT * FROM Ranking_CTE
WHERE Rank < 4

```
