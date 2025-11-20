## Scenario 
You learned how to find rows that contain the word "Laptop".
Now, your manager wants you to extract a specific piece of information from inside the text.
You have a column called Product_SKU (product code). This code is messy, but you know that the real product code always follows this pattern:
•	Two letters,
•	followed by a dash,
•	followed by four digits
(Example: AB-1234)
The challenge:
You must extract this pattern (XX-NNNN) from inside the text string.

## Pandas Solution
```python
# Pandas using jupyter
data = {
    'TransactionID': [1, 2, 3],
    'Notes': ['Sale OK. SKU is LP-1001. Ship fast.', 'SKU: MO-4055 (Urgent)', 'Defective item (SK-9902)']
}
df = pd.DataFrame(data)
pattern = r'([A-Z]{2}-[0-9]{4})'
df['ProductCode'] = df['Notes'].str.extract(pattern)
df[['TransactionID', 'ProductCode']]

```

## SQL Server Solution
```sql
SELECT TransactionID, 
SUBSTRING(Notes, PATINDEX('%[A-Z][A-Z]-[0-9][0-9][0-9][0-9]%', Notes), 7) AS Product_Code 
FROM dbo.Transactions


```
