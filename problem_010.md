## Scenario
You have an Employees table. The data is messy:
1.	The FullName column is stored as "LastName, FirstName" (e.g., "Hassan, Ali").
2.	The Location column is stored as "City, Country" (e.g., "Baghdad, Iraq").
The task: The HR department wants a "clean" report containing separate columns: FirstName, LastName, City, Country.

## Pandas Solution
```python
# Pandas using jupyter
data = {
    'FullName': ['Hassan, Ali', 'Saif, Noor', 'Ibrahim, Omar'],
    'Location': ['Baghdad, Iraq', 'Basra, Iraq', 'Erbil, Iraq']
}
df = pd.DataFrame(data)
df['FirstName'] = df['FullName'].str.split(',', expand=True)[0]
df['SecondName'] = df['FullName'].str.split(',', expand=True)[1]


```

## SQL Server Solution
```sql
SELECT SUBSTRING(FullName, 1, CHARINDEX(',',  FullName) - 1) FirstName
, SUBSTRING(FullName, CHARINDEX(',', FullName) + 1, LEN(FullName)) LastName
FROM dbo.Employees
```
