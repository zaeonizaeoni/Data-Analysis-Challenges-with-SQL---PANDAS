## Scenario
You have sales data, and there are ties in the numbers (more than one employee achieved the same sales).
The challenge: You are required to display 3 different types of ranking for the same data, to understand how each type handles ties


## Pandas Solution
```python
# Pandas using jupyter
data = {
    'Name': ['Ali', 'Sara', 'Hassan', 'Fatima', 'Zainab'],
    'Department': ['Sales', 'Sales', 'Support', 'Sales', 'Support'],
    'TotalSales_Text': ['1500', '1800', '1800', '1300', '1100'] # <-- نص
}
df = pd.DataFrame(data)
df['TotalSales_Text'] = df['TotalSales_Text'].str.strip().astype(int)
df = df.sort_values(by='TotalSales_Text', ascending=False)
df['R_DenseRank'] = df['TotalSales_Text'].rank(method='dense', ascending=False)
df['R_Rank'] = df['TotalSales_Text'].rank(method='min', ascending=False)
df['R_Num'] = df['TotalSales_Text'].rank(method='first', ascending=False)
df[['Name', 'TotalSales_Text', 'R_Num', 'R_Rank', 'R_DenseRank']]

```

## SQL Server Solution
```sql
WITH CleanedSalesPerformance AS (
SELECT *, CAST(TRIM(TotalSales_Text) AS INT) TotalSales
FROM dbo.SalesPerformance)
SELECT Name, TotalSales,
RANK() OVER(ORDER BY TotalSales DESC) R_Rank,
DENSE_RANK() OVER(ORDER BY TotalSales DESC)  R_DenseRank,
ROW_NUMBER() OVER(ORDER BY TotalSales DESC) R_Num
FROM CleanedSalesPerformance
```
