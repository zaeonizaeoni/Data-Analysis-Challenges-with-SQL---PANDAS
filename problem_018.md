## Scenario 
You have a single table: Employees. This table contains EmployeeID, EmpName, Salary, and most importantly, ManagerID (the ID of each employee’s manager).
The task: The HR department wants a report that compares each employee’s salary with the salary of their direct manager.

## Pandas Solution
```python
# Pandas using jupyter
data = {
    'EmployeeID': [1, 2, 3, 4, 5],
    'EmpName': ['Alice', 'Bob', 'Charlie', 'David', 'Eve'],
    'Salary': [6000, 7000, 5000, 8000, 9000],
    'ManagerID': [3, 3, 5, 5, None] # (Alice و Bob مديرهم Charlie)
                                      # (Charlie و David مديرهم Eve)
                                      # (Eve هي المدير الأعلى، ليس لها مدير)
}
df = pd.DataFrame(data)
df_copy = df.rename(columns={
    'EmployeeID':'ManID',
    'EmpName':'ManagerName',
    'Salary':'ManagerSalary'
})
merged_df = pd.merge(df, df_copy, how='inner', left_on='EmployeeID', right_on='ManagerID')
merged_df[['EmployeeID', 'EmpName', 'Salary', 'ManID', 'ManagerName', 'ManagerSalary']]


```

## SQL Server Solution
```sql
SELECT e.EmployeeID, e.EmpName, e.Salary, m.ManagerID, m.EmpName AS 'ManagerName', m.Salary AS 'ManagerSalary'
FROM dbo.Employees e
INNER JOIN dbo.Employees m ON e.EmployeeID = m.ManagerID

```
