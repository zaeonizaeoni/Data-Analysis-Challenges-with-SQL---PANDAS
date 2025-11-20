## Scenario
You have an Employees table. Due to a data entry error, some employees were entered with duplicate emails but different EmployeeIDs (perhaps an old ID and a new ID).The task: The HR department wants a "clean" report containing only one email per person. We need to "identify" duplicate rows (based on email) and "keep" the most recent row (the one with the highest EmployeeID).

## Pandas Solution
```python
# Pandas using jupyter
import pandas as pd

data = {
    'EmployeeID': [101, 102, 103, 104, 105],
    'Email': ['alice@company.com', 'bob@company.com', 'alice@company.com', 'charlie@company.com', 'bob@company.com']
}
df = pd.DataFrame(data)
df = df.sort_values(by='EmployeeID', ascending=False)
df_clean = df.drop_duplicates(subset=['Email'], keep='first')
df_clean

```

## SQL Server Solution
```sql
WITH  Clean_CTE AS (
SELECT EmployeeID, Email, ROW_NUMBER() OVER(PARTITION BY Email ORDER BY EmployeeID DESC) NN FROM dbo.Employees)
SELECT EmployeeID, Email FROM Clean_CTE
WHERE NN = 1


```
