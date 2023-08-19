# PostgresSQL-Cheatsheet

- **date-format: YYYY-MM-DD**
    - To separate date from DateTime column use `DATE(column_name)`

## SQL Fundamentals

### `SELECT` Statement

```sql
SELECT column_name_1,column_name_2 
FROM table_name;

SELECT * FROM table_name;
```

### `SELECT DISTINCT`

```sql
SELECT DISTINCT(column_name) 
FROM table_name;
```

### `COUNT`

```sql
SELECT COUNT(column_name) 
FROM table_name;

SELECT COUNT(*) 
FROM table_name;
```

### `WHERE` Statement

```sql
SELECT column_name_1,column_name_2 
FROM table_name 
WHERE column_name operator value;
```

| Operators |
| --- |
| = |
| > |
| < |
| >= |
| <= |
| <> or != |
| AND |
| OR |
| IN |
| BETWEEN (inclusive) |
| LIKE |
| IS NULL |
| NOT |
- **BETWEEN** is inclusive, **BETWEEN** value1 **AND** value2 is same as value ≥ low **AND** value ≤ high

```sql
BETWEEN 8 AND 9;
```

- **IN** operator is value **IN** (option1, option2…)

```sql
IN ('option','option')
```

- **LIKE**: (**%** and **_** ) and **ILIKE**
    - ********%  Matches any sequence of characters********
    - **_  Matches any single character**
    - **************************************************ILIKE is case insensitive.**************************************************

```sql
WHERE name LIKE 'A%';
WHERE name LIKE '%a';
WHERE name LIKE '_A_';
WHERE name LIKE 'a_';

```

### `ORDER BY` Statement

```sql
SELECT column_name_1,column_name_2 
FROM table_name 
ORDER BY column_name ASC/DESC;
```

### `LIMIT` Statement

```sql
SELECT column_name_1,column_name_2 
FROM table_name WHERE column_name operator value
LIMIT row_number;
```

## GROUP BY **statements**

### Aggregate Function

- **********`AVG(column_name)`**********
- **************`COUNT(column_name)`**************
- **********`MAX(column_name)`**********
- **********`MIN(column_name)`**********
- **********`SUM(column_name)`**********
- **************`ROUND(column_name, decimal value)`**************

```sql
SELECT AGGREGRATE() FROM table_name;
```

### `GROUP BY` Function

- Can be used on categorical columns, which are non-continuous.
- Example: Cabin class on a flight has (First Class, Business Class, Economy)
- These can be numerical or string.
- In the **************`SELECT`**  statement, column must wither have a **`AGGREGATE`** function or be in the  ****************`GROUP BY`** call.

```sql
SELECT category_column, AGGREGRATE(data_column)
FROM table
GROUP BY category_column;
```

- ****************`WHERE`**************** statements should not refer to the aggregation result.

```sql
SELECT category_column,category_column_2 AGGREGRATE(data_column)
FROM table
WHERE category_column_2 IN (option1,option2)
GROUP BY category_column,category_column_2;
```

- If you want to sort results based on the aggregate, make sure to reference the entire function.

```sql
SELECT category_column, AGGREGRATE(data_column)
FROM table
GROUP BY category_column
ORDER BY AGGREGATE(data_column);
```

### `HAVING` Function

- We can not use `WHERE` to filter based off aggregate results, because those happen after the a `WHERE` is executed.

```sql
SELECT category_column, AGGREGATE(data_column)
FROM table
WHERE category_column != 'value'
GROUP BY category_column
HAVING AGGREGATE(data_column) > 'value';
```

## JOINS Statements

- Creating an alias with the `AS` Clause
    - This alias gets executed at very end and hence cannot be used in `WHERE`, `HAVING` etc.
    
    ```sql
    SELECT column_name AS preferred_name
    FROM table_name
    WHERE column_name >= value;
    ```
    
- Types of `JOINS`
    - while selecting column if both tables have same column then you must mention table_name.column_name
        
        ```sql
        SELECT payment_id, payment.customer_id, first_name
        FROM payment 
        INNER JOIN customer
        ON payment.customer_id = customer.customer_id
        ```
        
    
    ### `INNER`
    
    ![`INNER JOIN`](PostgresSQL%20d6af1396d40c4e5f807423b5b03bfb3d/Untitled.png)
    
    `INNER JOIN`
    
    ```sql
    SELECT * FROM table_A
    INNER JOIN table_B
    ON table_A.column_match = table_b.column_match;
    ```
    
    ### `OUTER`
    
    - `FULL OUTER JOIN`
        
        ![Untitled](PostgresSQL%20d6af1396d40c4e5f807423b5b03bfb3d/Untitled%201.png)
        
        ```sql
        SELECT * FROM table_A
        FULL OUTER JOIN table_B
        ON table_A.column_match = table_b.column_match;
        ```
        
        R**emoving the intersection or Opposite of INNER JOIN**
        
        ```sql
        
        SELECT * FROM table_A
        FULL OUTER JOIN table_B
        ON table_A.column_match = table_b.column_match
        WHERE table_A.id IS null or table_B.id is null;
        ```
        
    - `LEFT OUTER JOIN` or `LEFT JOIN`
        
        ![Untitled](PostgresSQL%20d6af1396d40c4e5f807423b5b03bfb3d/Untitled%202.png)
        
        ```sql
        SELECT * FROM table_A
        LEFT OUTER JOIN table_B
        ON table_A.column_match = table_b.column_match;
        ```
        
        **Removing the intersection and need only the values of Left table.**
        
        ```sql
        #removing the intersection and need only the values of Left table.
        SELECT * FROM table_A
        LEFT OUTER JOIN table_B
        ON table_A.column_match = table_b.column_match
        WHERE table_B.id is null;
        ```
        
    - `RIGHT OUTER JOIN` or `RIGHT JOIN`
        
        ![Untitled](PostgresSQL%20d6af1396d40c4e5f807423b5b03bfb3d/Untitled%203.png)
        
        ```sql
        SELECT * FROM table_A
        RIGHT OUTER JOIN table_B
        ON table_A.column_match = table_b.column_match;
        ```
        
        **Removing the intersection and need only the values of Right table.**
        
        ```sql
        #removing the intersection and need only the values of Left table.
        SELECT * FROM table_A
        LEFT OUTER JOIN table_B
        ON table_A.column_match = table_b.column_match
        WHERE table_A.id IS null;
        ```
        
    
    ### `UNIONS`
    
    - When you want to combine tables into a one table
    
    ```sql
    SELECT * FROM table_A
    UNION
    SELECT * FROM table_B;
    ```
    
    **How to make multiple JOINS in a single query.**
    
    ```sql
    SELECT title, first_name, last_name
    FROM film_actor
    INNER JOIN actor
    ON film_actor.actor_id = actor.actor_id
    INNER JOIN film
    ON film_actor.film_id = film.film_id
    WHERE first_name='Nick'
    AND last_name = 'Wahlberg';
    ```
    

## ADVANCED SQL Commands

### Timestamps and `EXTRACT`
