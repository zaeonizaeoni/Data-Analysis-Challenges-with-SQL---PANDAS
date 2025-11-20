## Scenario 
You have a table of employees, including the birth date of each one.
The HR department wants to calculate the age (in years) of every employee as of today.
Task:
Create a report that shows the EmpID and a new column Age (the age in years).
(Note: The result depends on the "current day." Assume that “today” is November 10, 2025 for our calculations.)

## Pandas Solution
```python
# Pandas using jupyter
data = {
    'EmpID': [1, 2, 3],
    'DOB_Text': ['1990-05-15', '20-Mar-1985', '1995-11-01'] # DOB = Date of Birth
}
df = pd.DataFrame(data)
df['Date'] = pd.to_datetime(df['DOB_Text'], format='mixed')
today = pd.Timestamp.now()
age = today - df['Date']
df['Age'] = (age / pd.to_timedelta(365.25, 'D')).astype(int)
df[['EmpID', 'Age']]

```

## SQL Server Solution
```sql
DECLARE @Today DATE = GETDATE();
WITH CleanedEmployees_DOB_CTE AS (
SELECT	EmpID, CASE 
		WHEN CHARINDEX('-' ,DOB_Text) = 5 THEN CONVERT(DATE, DOB_Text, 23)
		WHEN CHARINDEX('-', DOB_Text) = 3 THEN CONVERT(DATE, DOB_Text, 106)
		ELSE NULL
END AS 'DOB_Date'
FROM dbo.Employees_DOB
)
SELECT EmpID, FLOOR(DATEDIFF(DAY, DOB_Date, @Today)/365) Age
FROM CleanedEmployees_DOB_CTE

```
