## Scenario 
You received the employee data, but the person who entered it was a bit "lazy."
Instead of putting the first name and last name in separate columns, they combined them into a single column (Full_Name).
Before you can use this data, you need to split this column into two columns: First_Name and Last_Name.

## Pandas Solution
```python
# Pandas using jupyter
data = {
    'EmpID': [1, 2, 3],
    'Full_Name': ['Ali Hassan', ' Sara Lee', 'Fatima Ahmed']
}
df = pd.DataFrame(data)
df['First_Name'] = df['Full_Name'].str.strip().str.split(' ').str[0]
df['Last_Name'] = df['Full_Name'].str.strip().str.split(' ').str[1]
df[['EmpID', 'First_Name', 'Last_Name']]

```

## SQL Server Solution
```sql
WITH Cleaned_Employees_Messy_CTE AS (
SELECT  EmpID, TRIM(Full_Name) FullName, 
		CHARINDEX(' ', TRIM(Full_Name)) Position
FROM dbo.Employees_Messy )

SELECT  EmpID, SUBSTRING(FullName, 1, Position - 1) FirstName,
		SUBSTRING(FullName, Position + 1, LEN(FullName)) SecondName
FROM Cleaned_Employees_Messy_CTE

```
