## Scenario
You have the same sensor data (every 15 minutes). The data is very "jumpy" – rising and falling quickly.
The manager does not want the "hourly" average (like in problem 81). He wants to see the overall "trend" of the data, while keeping the 15-minute interval. The task: The best way to "smooth" the data is to calculate a "moving average." We will create a new column where each row is the "average" of the last 4 readings (i.e., the average of the last hour)

## Pandas Solution
```python
# Pandas using jupyter
data = {
    'Timestamp': [
        '2025-11-10 09:00:00', '2025-11-10 09:15:00', '2025-11-10 09:30:00', '2025-11-10 09:45:00',
        '2025-11-10 10:00:00', '2025-11-10 10:15:00', '2025-11-10 10:30:00', '2025-11-10 10:45:00'
    ],
    'Temperature_C': [
        40.0, 40.2, 41.0, 40.8, # (الأرقام هنا مختلفة قليلاً للتوضيح)
        42.0, 42.5, 42.1, 43.0
    ]
}
df = pd.DataFrame(data)
df['Timestamp'] = pd.to_datetime(df['Timestamp'], format='%Y-%m-%d %H:%M:%S')
df = df.sort_values(by=['Timestamp', 'Temperature_C'])
df['AvgSmooth'] = df['Temperature_C'].rolling(window=4).mean()

```
