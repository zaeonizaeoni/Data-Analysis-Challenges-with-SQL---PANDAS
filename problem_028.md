## Scenario 
You received a sales report from the Accounting department. Unfortunately, they love Excel and sent you the data in a wide format (months as columns).
You cannot perform any analysis (like LAG or rolling calculations) on this format.
You need to unpivot it to convert it into a long format (one row per month).

## Pandas Solution
```python
# Pandas using jupyter
import pandas as pd
import numpy as np # نحتاج numpy لإدخال NULL

data = {
    'Employee': ['Ali', 'Ali', 'Sara', 'Sara'],
    'Product': ['Laptop', 'Mouse', 'Laptop', 'Mouse'],
    'Jan': [100, 20, 90, 22],
    'Feb': [120, 25, 110, np.nan] # <-- قيمة NULL (NaN)
}
df_wide = pd.DataFrame(data)

```

## SQL Server Solution
```sql
SELECT Employee, Product, 'Jan' AS 'Month', Jan AS 'Sales' FROM dbo.Sales_Wide_Multi
UNION ALL
SELECT Employee, Product, 'Feb' AS 'Month', Feb AS 'Sales' FROM dbo.Sales_Wide_Multi

```
