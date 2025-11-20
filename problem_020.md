## Scenario 
You have a log of user clicks on a website. A “session” is defined as follows:
•	A sequence of clicks made by the same user.
•	The time difference between any two consecutive clicks must be less than 10 minutes.
•	If the user waits more than 10 minutes, the next click starts a new session.
Hard Mode: You are required to assign a unique SessionID for each session, for each user.

## Pandas Solution
```python
# Pandas using jupyter
import pandas as pd
data = {
    'UserID': [1, 1, 1, 1, 2, 2, 1, 1, 1],
    'ClickTime': [
        '2025-11-10 09:00:00', # User 1, Session 1, Click 1
        '2025-11-10 09:02:00', # User 1, Session 1, Click 2 (<10 min)
        '2025-11-10 09:05:00', # User 1, Session 1, Click 3 (<10 min)
        '2025-11-10 09:17:00', # User 1, Session 2, Click 1 (>10 min gap)
        '2025-11-10 10:00:00', # User 2, Session 1, Click 1
        '2025-11-10 10:05:00', # User 2, Session 1, Click 2 (<10 min)
        '2025-11-10 09:18:00', # User 1, Session 2, Click 2 (<10 min)
        '2025-11-10 09:30:00', # User 1, Session 3, Click 1 (>10 min gap)
        '2025-11-10 09:31:00', # User 1, Session 3, Click 2 (<10 min)
    ]
}
df = pd.DataFrame(data)
df['ClickTime'] = pd.to_datetime(df['ClickTime'])
# (مهم جداً: يجب فرز البيانات أولاً حسب المستخدم والوقت)
df = df.sort_values(by=['UserID', 'ClickTime']).reset_index(drop=True)
df = df.sort_values(by=['UserID', 'ClickTime']).reset_index(drop=True)
df['TimeGap'] = df.groupby('UserID')['ClickTime'].diff()
df['IsNewSession'] = (
    (df['TimeGap'].isnull()) |
    (df['TimeGap'] > pd.to_timedelta('10 minutes'))
).astype(int)
df['SessionID'] = df.groupby('UserID')['IsNewSession'].cumsum()
df[['UserID', 'ClickTime', 'TimeGap', 'SessionID']]

```

## SQL Server Solution
```sql
WITH TimeGaps_CTE AS (SELECT 
    UserID,
    ClickTime,
    PreviousClickTime,
    Dur,
    condition,
    SUM(condition) OVER(PARTITION BY UserID ORDER BY UserID, ClickTime) SessionID
FROM Cond_CTE


    SELECT
        UserID,
        ClickTime,
        LAG(ClickTime, 1) OVER(PARTITION BY UserID ORDER BY ClickTime) PreviousClickTime
    FROM dbo.UserClicks 
),
DiffClickTime_CTE AS (
    SELECT 
        UserID,
        ClickTime,
        PreviousClickTime,
        DATEDIFF(MINUTE, PreviousClickTime, ClickTime) Dur
    FROM TimeGaps_CTE
),
Cond_CTE AS (
    SELECT
        UserID,
        ClickTime,
        PreviousClickTime,
        Dur,
        CASE 
            WHEN Dur IS NULL THEN 1
            WHEN Dur >  10 THEN 1
            ELSE 0
        END AS 'condition'
    FROM DiffClickTime_CTE
)
SELECT 
    UserID,
    ClickTime,
    PreviousClickTime,
    Dur,
    condition,
    SUM(condition) OVER(PARTITION BY UserID ORDER BY UserID, ClickTime) SessionID
FROM Cond_CTE
```
