## Scenario
We have the same Employees table from problem 77. This table shows the company's "hierarchy" (who manages whom).
EmployeeID	empname	 managerID
  1       	Alice	    3
  2	        Bob	        3
  3	        Charlie 	5
  4	        David   	5
  5     	Eve	      null
The task:
•	In problem 77 (Self-Join), we found the "direct manager."
•	Now, the top manager (Eve) wants to know the "full chain of command" for employee "Alice."
•	We want to see: Alice (managed by) Charlie (managed by) Eve


## SQL Server Solution
```sql
WITH Heirarchy_CTE AS (
SELECT EmployeeID, EmpName, ManagerID, 0 AS HierarchyLevel FROM Employees
WHERE EmployeeID = 1
UNION ALL
SELECT e.EmployeeID, e.EmpName, e.ManagerID, h.HierarchyLevel + 1 FROM Employees e
INNER  JOIN Heirarchy_CTE h ON e.EmployeeID = h.ManagerID
)
SELECT * FROM Heirarchy_CTE

```
