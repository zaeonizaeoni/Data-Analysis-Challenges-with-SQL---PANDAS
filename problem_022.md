## Scenario 
You have a log of all sales, containing multiple sales entries for each employee on different dates.

## Pandas Solution
```python
# Pandas using jupyter
data = {
    'SaleID': [1, 2, 3, 4, 5, 6, 7],
    'EmployeeID': [101, 102, 101, 103, 102, 101, 103],
    'SaleDate': ['2025-11-01', '2025-11-01', '2025-11-03', '2025-11-02', '2025-11-04', '2025-11-02', '2025-11-05'],
    'SaleAmount': [500, 600, 550, 400, 650, 520, 450]
}
df = pd.DataFrame(data)
df['SaleDate'] = pd.to_datetime(df['SaleDate'])
df['Rank'] = df.groupby('EmployeeID')['SaleDate'].rank(ascending=False, method='dense')
df[df['Rank'] == 1][['SaleID', 'EmployeeID', 'SaleDate', 'SaleAmount']]
```

## SQL Server Solution
```sql
WITH CleanedSaleslog_CTE AS (
	SELECT 
		SaleID, 
		EmployeeID, 
		CONVERT(DATE, SaleDate, 23) SaleDate, 
		SaleAmount 
	FROM dbo.Saleslog),
RankSaleslog_CTE AS (
	SELECT *, 
		RANK() OVER(PARTITION BY EmployeeID ORDER BY SaleDate DESC) Ranking 
		FROM CleanedSaleslog_CTE
)
SELECT SaleID, EmployeeID, SaleDate, SaleAmount FROM RankSaleslog_CTE
WHERE Ranking = 1
```
