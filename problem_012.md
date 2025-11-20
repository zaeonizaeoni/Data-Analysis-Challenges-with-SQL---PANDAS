## Scenario
You have the Sales table that we used previously (from problem 75). The Marketing department wants a report that does not show the exact "Amount," but instead contains a "Category" for each sale.
The required categories are:
•	If Amount > 500, the category is 'High'.
•	If Amount > 200 (but <= 500), the category is 'Medium'.
•	If Amount <= 200, the category is 'Low'.

## Pandas Solution
```python
# Pandas using jupyter
import numpy as np

data_sales = {
    'SaleID': [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12],
    'EmployeeID': [101, 101, 102, 102, 103, 103, 104, 104, 104, 105, 105, 106],
    'Amount': [500, 550, 600, 650, 400, 450, 100, 150, 200, 300, 350, 800]
}
df_sales = pd.DataFrame(data_sales)
conditions = [(df_sales['Amount'] > 500),
              (df_sales['Amount'] > 200),
              (df_sales['Amount'] <= 200)]
choices = ['High', 'Medimum', 'Low']
df_sales['Category'] = np.select(conditions, choices, default='Other')

```

## SQL Server Solution
```sql
SELECT SaleID, EmployeeID, Amount,
CASE 
    WHEN Amount > 500 THEN 'High'
    WHEN Amount > 200 THEN 'Medium'
    WHEN Amount <= 200 THEN 'Low'
    ELSE 'XD'
END AS  'Category'
FROM dbo.Sales

```
