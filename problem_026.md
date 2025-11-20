## Scenario 
You have a table of product codes. The old system used a hyphen (-) to separate the product category from its number (e.g., LAP-1001). The new system requires using an underscore (_) as the separator (e.g., LAP_1001).
The challenge:
You need to replace all hyphens (-) with underscores (_) in the product codes column.

## Pandas Solution
```python
# Pandas using jupyter
import pandas as pd

data = {
    'ProductID': [1, 2, 3],
    'SKU': ['LAP-1001', ' MOU-1002 ', 'SCRN-1003'] 
}
df = pd.DataFrame(data)
df['SKU'] = df['SKU'].str.strip().str.replace('-', '_')

```

## SQL Server Solution
```sql
SELECT ProductID, REPLACE(TRIM(SKU), '-', '_') SKU FROM dbo.Products

```
