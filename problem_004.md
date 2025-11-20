## Scenario
“You have the same Houses table, which contains Price, a continuous number. The problem: Sometimes, the exact number is not important. Your manager wants a simple report that classifies houses into price bins. The task: You need to convert the continuous number (Price) into a categorical variable (Price_Category)

## Pandas Solution
```python
# Pandas using jupyter
import pandas as pd

data = {
    'HouseID': [1, 2, 3, 4, 5],
    'Region': ['North', 'South', 'North', 'East', 'South'],
    'Price': [200, 300, 210, 250, 350] # (Low, Medium, Low, Medium, High)
}
df = pd.DataFrame(data)
bins = [-float('inf'), 210, 300, float('inf')]
labels = ['Low', 'Medium', 'High']
df['PriceCategory'] = pd.cut(df['Price'], bins=bins, labels=labels, right=True)

```

## SQL Server Solution
```sql
SELECT HouseID, Price,
CASE WHEN Price <= 210 THEN 'Low'
	 WHEN Price <= 300 THEN 'Medium'
	 ELSE 'High'
END AS 'Price_Category'
FROM dbo.Houses
```
