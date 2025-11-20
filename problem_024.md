## Scenario 
You work as a civil engineer in a company, and your manager has asked you to analyze the duration of projects (how many years each project took).
The problem is that the start and end dates in the system were entered manually by different engineers, and the data is very messy, containing American formats (mm/dd), British formats (dd/mm), and mixed formats.

## Pandas Solution
```python
# Pandas using jupyter
data = {
    'ProjectID': ['A', 'B', 'C', 'D'],
    'StartDate_Text': ['01/10/2020', '2021-03-15', '5-May-2019', '02/20/2020'], # بريطاني، ISO، مختلط، أمريكي
    'EndDate_Text': ['20/12/2023', '2024-01-20', '15/05/2022', 'Bad Data'] # بريطاني، ISO، بريطاني، بيانات تالفة
}
df = pd.DataFrame(data)
df['StartDate'] = pd.to_datetime(df['StartDate_Text'], format='mixed', dayfirst=True, errors='coerce')
df['EndDate'] = pd.to_datetime(df['EndDate_Text'], format='mixed', dayfirst=True, errors='coerce')
df['Duration'] = (df['EndDate'] - df['StartDate']) / pd.to_timedelta(365.25, 'D')
df[['ProjectID', 'Duration']]


```

## SQL Server Solution
```sql
WITH CleanedProject_Dates_CTE AS (
	SELECT
		ProjectID,
		COALESCE(
			TRY_CONVERT(DATE, StartDate_Text, 23),
			--TRY_CONVERT(DATE, StartDate_Text, 101),
			TRY_CONVERT(DATE, StartDate_Text, 103),
			TRY_CONVERT(DATE, StartDate_Text, 105),
			TRY_CONVERT(DATE, StartDate_Text, 110),
			TRY_CONVERT(DATE, StartDate_Text, 111)
		) StartDate,
		COALESCE(
			TRY_CONVERT(DATE, EndDate_Text, 23),
			--TRY_CONVERT(DATE, EndDate_Text, 101),
			TRY_CONVERT(DATE, EndDate_Text, 103),
			TRY_CONVERT(DATE, EndDate_Text, 105),
			TRY_CONVERT(DATE, EndDate_Text, 110),
			TRY_CONVERT(DATE, EndDate_Text, 111)
		) EndDate
	FROM dbo.Project_Dates
)
SELECT ProjectID, CAST(DATEDIFF(DAY, StartDate, EndDate)/365.25 AS DECIMAL(10,3)) Duration FROM CleanedProject_Dates_CTE

COALESCE(
			TRY_CONVERT(DATE, EndDate_Text, 23),
			--TRY_CONVERT(DATE, EndDate_Text, 101),
			TRY_CONVERT(DATE, EndDate_Text, 103),
			TRY_CONVERT(DATE, EndDate_Text, 105),
			TRY_CONVERT(DATE, EndDate_Text, 110),
			TRY_CONVERT(DATE, EndDate_Text, 111)
		) EndDate
	FROM dbo.Project_Dates
)
SELECT ProjectID, CAST(DATEDIFF(DAY, StartDate, EndDate)/365.25 AS DECIMAL(10,3)) Duration FROM CleanedProject_Dates_CTE


```
