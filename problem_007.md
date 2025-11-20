## Scenario
. You have a SurveyResults table. Users entered the "JobTitle" manually. The problem: The data is very messy. "Civil Engineer", "civil engineer" (case difference), and "Civil Eng." (abbreviation) all mean the same thing. The task: We want a "clean" report that "standardizes" all these variations into a single value: "Civil Engineer".”

## Pandas Solution
```python
# Pandas using jupyter
import pandas as pd
import numpy as np

data = {
    'SurveyID': [1, 2, 3, 4, 5],
    'JobTitle': ['Civil Engineer', 'civil engineer', 'Steel Inspector', 'Civil Eng.', 'Site Manager']
}
df = pd.DataFrame(data)
conditions = [df['JobTitle'].str.lower().str.contains('civil eng')]
choices = ['Civil Engineer']
df['JobTitlec'] = np.select(conditions, choices, df['JobTitle'])
```

## SQL Server Solution
```sql
SELECT SurveyID, JobTitle, 
	CASE
		WHEN LOWER(JobTitle) LIKE '%civil eng%' THEN 'Civil Engineer'
		ELSE JobTitle
	END AS 'JobTitleC'
FROM dbo.SurveyResults

```
