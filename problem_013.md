## Scenario
You are analyzing stock data for a major construction company. You have a simple table that records the daily "Close Price."
•	Your manager wants a report showing the "Percentage Change" from day to day.

## Pandas Solution
```python
# Pandas using jupyter
data = {
    'Date': ['2025-11-03', '2025-11-04', '2025-11-05', '2025-11-06', '2025-11-07'],
    'Close_Price': [100.0, 102.0, 101.0, 105.0, 105.0]
}
df = pd.DataFrame(data)
df['Date'] = pd.to_datetime(df['Date'])
df = df.sort_values(by='Date')
# df['Day_over_Day_Changes'] = ( df['Close_Price'] - (df['Close_Price'].shift(1)) ) / (df['Close_Price'].shift(1))
df['Day_over_Day_Changes'] = df['Close_Price'].pct_change()

```

