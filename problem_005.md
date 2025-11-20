## Scenario
“You are preparing the data for a Machine Learning model to predict house prices. The problem: Mathematical models (like linear regression) do not understand text (e.g., 'North', 'South'). They only understand numbers. The task: You need to convert categorical data (e.g., Region) into numerical data that the model can understand.”

## Pandas Solution
```python
# Pandas using jupyter
import pandas as pd

data = {
    'HouseID': [1, 2, 3, 4],
    'Region': ['North', 'South', 'North', 'East'],
    'Price': [200, 300, 210, 250]
}
df = pd.DataFrame(data)
df_encoded = pd.get_dummies(
    df,
    columns=['Region'],
    prefix='Region'
).astype(int)
df_encoded
```

## SQL Server Solution
```sql
SELECT HouseID, Price,
	CASE WHEN Region = 'North' THEN 1 ELSE 0 END AS Region_North,
	CASE WHEN Region = 'South' THEN 1 ELSE 0 END AS Region_South,
	CASE WHEN Region = 'East' THEN 1 ELSE 0 END AS Region_East
FROM dbo.Houses
```
