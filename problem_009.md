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

df_customers = pd.DataFrame({
    'CustID': [1, 2, 3, 4],
    'CustName': ['Ali', 'Noor', 'Omar', 'Sara']
})

df_orders_curr = pd.DataFrame({
    'OrderID': [101, 102],
    'CustID': [1, 2],
    'Amount': [500, 600]
})

df_orders_arch = pd.DataFrame({
    'OrderID': [98, 99],
    'CustID': [1, 3],
    'Amount': [100, 200]
})
df_con = pd.concat([df_orders_curr, df_orders_arch], ignore_index=True)
All_details = pd.merge(df_customers, df_con, on='CustID', how='left')
All_details

```

## SQL Server Solution
```sql
WITH Concating_CTE AS (
    SELECT * FROM dbo.Orders_Current
    UNION ALL 
    SELECT * FROM dbo.Orders_Archived
)
SELECT * FROM dbo.Customers c
LEFT JOIN Concating_CTE cc ON c.CustID = cc.CustID

```
