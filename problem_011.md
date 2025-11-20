## Scenario
You have a Products table. Some products have a "Sale_Price," while others do not (its value is NULL).
The Accounting department wants a report showing the "actual price" the customer will pay:
•	If Sale_Price exists (is not NULL), use it.
•	If Sale_Price is empty (NULL), use the Regular_Price.

## Pandas Solution
```python
# Pandas using jupyter
import numpy as np # (سنحتاج numpy لـ np.nan)
data = {
    'Product': ['Laptop', 'Mouse', 'Keyboard', 'Monitor'],
    'Regular_Price': [1200, 80, 150, 400],
    'Sale_Price': [1000, np.nan, 120, np.nan] # (Mouse و Monitor ليس لهما سعر تخفيض)
}
df = pd.DataFrame(data)
df['FinalPrice'] = df['Sale_Price'].fillna(df['Regular_Price'])
df[['Product', 'FinalPrice']]

```

## SQL Server Solution
```sql
SELECT Product, COALESCE(Sale_Price, Regular_Price) FinalPrice FROM dbo.Products
```
