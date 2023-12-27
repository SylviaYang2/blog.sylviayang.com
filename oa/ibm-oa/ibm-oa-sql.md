# IBM OA SQL

<figure><img src="../../.gitbook/assets/image (25) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (26) (1).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```sql
SELECT 
    a.username,
    a.email,
    COUNT(ai.item_id) AS items,
    SUM(i.weight) AS total_weight
FROM 
    accounts a
JOIN 
    accounts_items ai ON a.id = ai.account_id
JOIN 
    items i ON ai.item_id = i.id
GROUP BY 
    a.id
HAVING 
    SUM(i.weight) > 20
ORDER BY 
    total_weight DESC, 
    a.username ASC;
```
{% endcode %}





<figure><img src="../../.gitbook/assets/image (20) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (21) (1).png" alt=""><figcaption></figcaption></figure>

```sql
SELECT 
    c.name AS city,
    COUNT(b.id) AS banners,
    MIN(b.width * b.height) AS min_area,
    CEILING(AVG(b.width * b.height)) AS avg_area,
    MAX(b.width * b.height) AS max_area,
    SUM(b.width * b.height) AS total_area
FROM 
    cities c
JOIN 
    banners b ON c.id = b.city_id
GROUP BY 
    c.name
ORDER BY 
    c.name ASC;
```



<figure><img src="../../.gitbook/assets/image (22) (1).png" alt="" width="375"><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (23) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (28) (1).png" alt=""><figcaption></figcaption></figure>

{% code overflow="wrap" %}
```sql
To write a SQL query that returns the list of IBANs with their corresponding total of positive and negative transactions, we'll need to use a `CASE` statement to separate the positive and negative amounts, because the transaction amounts are in a string format with parentheses indicating negative amounts.

Assuming that the amounts are stored as strings and negative amounts are enclosed in parentheses like '(100)', here is a query that could accomplish this:
```
{% endcode %}

{% code overflow="wrap" %}
````sql
SELECT 
    ac.iban,
    COALESCE(SUM(CASE WHEN tr.amount LIKE '(%' THEN CAST(REPLACE(REPLACE(tr.amount, '(', ''), ')', '') AS DECIMAL(10, 2)) ELSE 0 END), 0) AS debit,
    COALESCE(SUM(CASE WHEN tr.amount NOT LIKE '(%' THEN CAST(tr.amount AS DECIMAL(10, 2)) ELSE 0 END), 0) AS credit
FROM 
    accounts ac
LEFT JOIN 
    transactions tr ON ac.id = tr.account_id
GROUP BY 
    ac.iban
ORDER BY 
    ac.iban;
```

This query does the following:

1. Joins the `accounts` table with the `transactions` table on the account ID.
2. Uses a `CASE` statement within the `SUM` function to determine whether the transaction amount is negative (indicated by parentheses) and converts it to a negative number by removing the parentheses and casting the amount to a decimal. If it's not negative, it sums up the value directly.
3. Groups the results by the IBAN.
4. Orders the results in ascending order by the IBAN.
5. Uses `COALESCE` to handle cases where there might be no transactions, defaulting the sum to 0.

Please note that this query assumes the parentheses are used in the `amount` column to indicate negative values. The `REPLACE` function is used to remove the parentheses before casting the string to a number. The `CAST` function is used to convert the string representation of the number into a decimal type, which allows for the summing of amounts.
````
{% endcode %}



<figure><img src="../../.gitbook/assets/image (27) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (29) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (30) (1).png" alt=""><figcaption></figcaption></figure>

```sql
SELECT NAME 
FROM HACKER 
WHERE (MONTHS < 10 AND (MONTHS * HACKOS) > 100)
ORDER BY ID ASC;
```

