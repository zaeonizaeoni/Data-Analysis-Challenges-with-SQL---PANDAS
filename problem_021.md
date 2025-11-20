## Scenario 
You have the same SalesLog for employees. In the previous problem, we wanted the “latest” sale.
Now, the Accounting department wants a different report. They want to see the “growth” of each employee’s sales over time. They want a new column that shows the running total of each employee’s sales up to that day.

## Pandas Solution
```python
# Pandas using jupyter
data = {
    'EmployeeID': [101, 101, 101, 102, 102],
    'SaleDate': ['2025-11-01', '2025-11-03', '2025-11-05', '2025-11-02', '2025-11-04'],
    'SaleAmount': [100, 150, 300, 200, 50]
}
df = pd.DataFrame(data)
df['SaleDate'] = pd.to_datetime(df['SaleDate'])
df = df.sort_values(by=['EmployeeID', 'SaleDate'])

```

## SQL Server Solution
```sql
df['RunningTotal'] = df.groupby('EmployeeID')['SaleAmount'].cumsum()

```
