## Scenario
Scenario 
You have a Products table. Instead of having 50 separate specification columns (such as color, weight, warranty), the company has compressed all these specifications into a single JSON column called Specs.
The task: Your manager wants a report that shows the product name, the product price, and most importantly, the "Warranty" and "Color" extracted from inside the JSON column.

## Pandas Solution
```python
# Pandas using jupyter
import json

data = {
    'ProductID': [1, 2, 3],
    'ProductName': ['Laptop', 'Mouse', 'Headphones'],
    'Price': [1200, 80, 150],
    # (هذا هو عمود الـ JSON كنص)
    'Specs': [
        '{"Color": "Silver", "Warranty": "2 Years", "Weight": "1.5kg"}',
        '{"Color": "Black", "Warranty": "1 Year", "Wireless": true}',
        '{"Color": "White", "Warranty": "1 Year", "NoiseCancelling": true}'
    ]
}
df = pd.DataFrame(data)
df_cc = df['Specs'].apply(json.loads)
df_cc = pd.json_normalize(df_cc)
df_f = pd.concat([df[['ProductID', 'ProductName', 'Price']], df_cc], axis=1)
df_f[['ProductName', 'Price', 'Color', 'Warranty']]

```

## SQL Server Solution
```sql
SELECT ProductName, Price, JSON_VALUE(Specs, '$.Color') Color, JSON_VALUE(Specs, '$.Warranty') Warranty FROM dbo.Products
```
