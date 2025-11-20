## Scenario
You have 3 tables:
1.	Customers: the list of customers (the main table).
2.	Orders_Current: the "active" orders (for this year).
3.	Orders_Archived: the "old" orders (from the archive).
The task: The manager wants a single report that shows all customers (Customers) and all their orders (whether Current or Archived) in one report.

## Pandas Solution
```python
# Pandas using jupyter
import pandas as pd

# Let assume this data is very big
data = {
    'LogID': range(1, 101),
    'UserID': [f'User_{i}' for i in range(1, 101)],
    'Page': ['PageA', 'PageB', 'PageC'] * 33 + ['PageA']
}
df = pd.DataFrame(data)
df.head(10)
df_sample = df.sample(frac=0.1)
df_sample

```

## SQL Server Solution
```sql
SELECT TOP(10) PERCENT * FROM dbo.Website_Logs
ORDER BY NEWID()

```
