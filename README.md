# PostgreSQL
## Table of Contents:
- [SQL Fundamentals](#sql-fundamentals)
- [GROUP BY statements](#group-by-statements)
- [JOINS Statements](#joins-statements)
- [ADVANCED SQL Commands](#advanced-sql-commands)
- [CREATE databases and Tables](#create-databases-and-tables)
- [Conditional Expressions and Procedures Introduction](#conditional-expressions-and-procedures-introduction)


## Things To Remember:

1. **date-format: YYYY-MM-DD**
   - To separate date from DateTime column use `DATE(column_name)`
2. CURRENT_TIMESTAMP gives current time, which can be used to insert.

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

| Operators           |
| ------------------- |
| =                   |
| >                   |
| <                   |
| >=                  |
| <=                  |
| <> or !=            |
| AND                 |
| OR                  |
| IN                  |
| BETWEEN (inclusive) |
| LIKE                |
| IS NULL             |
| NOT                 |

- **BETWEEN** is inclusive, **BETWEEN** value1 **AND** value2 is same as value ≥ low **AND** value ≤ high

```sql
BETWEEN 8 AND 9;
```

- **IN** operator is value **IN** (option1, option2…)

```sql
IN ('option','option')
```

- **LIKE**: (**%** and **\_** ) and **ILIKE**
  - **\*\*\*\***% Matches any sequence of characters**\*\*\*\***
  - **\_ Matches any single character**
  - ************************\*\*************************ILIKE is case insensitive.************************\*\*************************

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

- ****\*\*****`AVG(column_name)`****\*\*****
- ******\*\*******`COUNT(column_name)`******\*\*******
- ****\*\*****`MAX(column_name)`****\*\*****
- ****\*\*****`MIN(column_name)`****\*\*****
- ****\*\*****`SUM(column_name)`****\*\*****
- ******\*\*******`ROUND(column_name, decimal value)`******\*\*******

```sql
SELECT AGGREGRATE() FROM table_name;
```

### `GROUP BY` Function

- Can be used on categorical columns, which are non-continuous.
- Example: Cabin class on a flight has (First Class, Business Class, Economy)
- These can be numerical or string.
- In the ******\*\*******`SELECT`** statement, column must wither have a **`AGGREGATE`** function or be in the ******\*\*********`GROUP BY`\*\* call.

```sql
SELECT category_column, AGGREGRATE(data_column)
FROM table
GROUP BY category_column;
```

- ******\*\*\*\*******`WHERE`******\*\*\*\******* statements should not refer to the aggregation result.

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
  ![`INNER JOIN`](PostgreSQL%20d6af1396d40c4e5f807423b5b03bfb3d/Untitled.png)
  `INNER JOIN`
  ```sql
  SELECT * FROM table_A
  INNER JOIN table_B
  ON table_A.column_match = table_b.column_match;
  ```
  ### `OUTER`
  - `FULL OUTER JOIN`
    ![Untitled](PostgreSQL%20d6af1396d40c4e5f807423b5b03bfb3d/Untitled%201.png)
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
    ![Untitled](PostgreSQL%20d6af1396d40c4e5f807423b5b03bfb3d/Untitled%202.png)
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
    ![Untitled](PostgreSQL%20d6af1396d40c4e5f807423b5b03bfb3d/Untitled%203.png)
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

# Timestamps contains Time, Date and can even contain timezone, So as to extract certain components we use extract, which are

- `YEAR`
- `MONTH`
- `DAY`
- `WEEK`
- `QUATER`

```sql
SELECT EXTRACT(YEAR FROM date_column)
FROM table_name
```

### Timestamps and `AGE`

# Calculates and returns current age of a given timestamp

```sql
SELECT AGE(date_column)
FROM table_name
```

### Timestamps and `TO_CHAR`

# Calculates and returns current age of a given timestamp

```sql
SELECT TO_CHAR(date_column,'_date_format')
FROM table_name
```

[**https://www.postgresql.org/docs/current/functions-formatting.html**](https://www.postgresql.org/docs/current/functions-formatting.html)

for all types of date time formatting refer the above documentation.

### Mathematical Functions

[**https://www.postgresql.org/docs/9.5/functions-math.html**](https://www.postgresql.org/docs/9.5/functions-math.html)

link to all the mathematical functions available in postgreSQL

```sql
--Example: We need to find area of rectangle given that length and
				-- breath has a column

SELECT column_length * column_breath FROM table_name
```

### String Functions and Operators

[**https://www.postgresql.org/docs/9.1/functions-string.html**](https://www.postgresql.org/docs/9.1/functions-string.html)

link to all the string functions available in postgreSQL

```sql
SELECT column_length || column_breath FROM table_name

-- || is used to concatenate
```

### SubQuery

- The subQuery is performed first since it is inside the paranthesis.
- We can also use the `IN` operator in conjunction with a subquery to check against multiple results returned.

```sql
SELECT column_A, column_B
FROM table_A
WHERE **column_A** in (SELECT **column_A FROM table_B)**
```

- if the subquery does not have any values then the above code can run into issues.
- hence, we use `EXISTS` to check and run the query error free.

```sql
SELECT column_A, column_B
FROM table_A as A
WHERE EXISTS
(SELECT * FROM table_b as B
 WHERE B.id = A.id)
```

```sql
--EXAMPLE:
SELECT
	film_id,
	title
FROM
	film
WHERE
	film_id IN (
		SELECT
			inventory.film_id
		FROM
			rental
		INNER JOIN inventory ON inventory.inventory_id = rental.inventory_id
		WHERE
			return_date BETWEEN '2005-05-29'
		AND '2005-05-30'
	);
```

### `SELF JOIN`

A self-join is a query in which a table is joined to itself.

Self-Joins are useful for comparing values in a column of rows within the same table

```sql
SELECT table_A.col, table_B.col
FROM table as table_A
JOIN table as table_B
ON table_A.some_col = table_B.other_col
```

```sql
Example:
SELECT table_A.name as name, table_b.name as report
FROM EMPLOYEES as table_A
JOIN EMPLOYEES as table_B
ON table_A.emp_id = table_B.report_id
```

## CREATE databases and Tables

### Data Types

Reference from [https://www.tutorialspoint.com/postgresql/](https://www.tutorialspoint.com/postgresql/)

### **Numeric Types**

Numeric types consist of two-byte, four-byte, and eight-byte integers, four-byte and eight-byte floating-point numbers, and selectable-precision decimals. The following table lists the available types.

| Name             | Storage Size | Description                    | Range                                                                                    |
| ---------------- | ------------ | ------------------------------ | ---------------------------------------------------------------------------------------- |
| smallint         | 2 bytes      | small-range integer            | -32768 to +32767                                                                         |
| integer          | 4 bytes      | typical choice for integer     | -2147483648 to +2147483647                                                               |
| bigint           | 8 bytes      | large-range integer            | -9223372036854775808 to 9223372036854775807                                              |
| decimal          | variable     | user-specified precision,exact | up to 131072 digits before the decimal point; up to 16383 digits after the decimal point |
| numeric          | variable     | user-specified precision,exact | up to 131072 digits before the decimal point; up to 16383 digits after the decimal point |
| real             | 4 bytes      | variable-precision,inexact     | 6 decimal digits precision                                                               |
| double precision | 8 bytes      | variable-precision,inexact     | 15 decimal digits precision                                                              |
| smallserial      | 2 bytes      | small autoincrementing integer | 1 to 32767                                                                               |
| serial           | 4 bytes      | autoincrementing integer       | 1 to 2147483647                                                                          |
| bigserial        | 8 bytes      | large autoincrementing integer | 1 to 9223372036854775807                                                                 |

### **Monetary Types**

The *money* type stores a currency amount with a fixed fractional precision. Values of the *numeric, int, and bigint* data types can be cast to *money*. Using Floating point numbers is not recommended to handle money due to the potential for rounding errors.

| Name  | Storage Size | Description     | Range                                          |
| ----- | ------------ | --------------- | ---------------------------------------------- |
| money | 8 bytes      | currency amount | -92233720368547758.08 to +92233720368547758.07 |

### **Character Types**

The table given below lists the general-purpose character types available in PostgreSQL.

| S. No.                     | Name & Description               |
| -------------------------- | -------------------------------- |
| 1                          | character varying(n), varchar(n) |
| variable-length with limit |
| 2                          | character(n), char(n)            |
| fixed-length, blank padded |
| 3                          | text                             |
| variable unlimited length  |

### **Binary Data Types**

The *bytea* data type allows storage of binary strings as in the table given below.

| Name  | Storage Size                               | Description                   |
| ----- | ------------------------------------------ | ----------------------------- |
| bytea | 1 or 4 bytes plus the actual binary string | variable-length binary string |

### **Date/Time Types**

PostgreSQL supports a full set of SQL date and time types, as shown in table below. Dates are counted according to the Gregorian calendar. Here, all the types have resolution of **1 microsecond / 14 digits** except **date** type, whose resolution is **day**.

| Name                                 | Storage Size | Description                        | Low Value        | High Value      |
| ------------------------------------ | ------------ | ---------------------------------- | ---------------- | --------------- |
| timestamp [(p)] [without time zone ] | 8 bytes      | both date and time (no time zone)  | 4713 BC          | 294276 AD       |
| TIMESTAMPTZ                          | 8 bytes      | both date and time, with time zone | 4713 BC          | 294276 AD       |
| date                                 | 4 bytes      | date (no time of day)              | 4713 BC          | 5874897 AD      |
| time [ (p)] [ without time zone ]    | 8 bytes      | time of day (no date)              | 00:00:00         | 24:00:00        |
| time [ (p)] with time zone           | 12 bytes     | times of day only, with time zone  | 00:00:00+1459    | 24:00:00-1459   |
| interval [fields ] [(p) ]            | 12 bytes     | time interval                      | -178000000 years | 178000000 years |

### **Boolean Type**

PostgreSQL provides the standard SQL type Boolean. The Boolean data type can have the states *true*, *false*, and a third state, *unknown*, which is represented by the SQL null value.

| Name    | Storage Size | Description            |
| ------- | ------------ | ---------------------- |
| boolean | 1 byte       | state of true or false |

### **Enumerated Type**

Enumerated (enum) types are data types that comprise a static, ordered set of values. They are equivalent to the enum types supported in a number of programming languages.

Unlike other types, Enumerated Types need to be created using CREATE TYPE command. This type is used to store a static, ordered set of values. For example compass directions, i.e., NORTH, SOUTH, EAST, and WEST or days of the week as shown below −

```
CREATE TYPE week AS ENUM ('Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun');
```

Enumerated, once created, can be used like any other types.

### **Geometric Type**

Geometric data types represent two-dimensional spatial objects. The most fundamental type, the point, forms the basis for all of the other types.

| Name    | Storage Size | Representation                        | Description                         |
| ------- | ------------ | ------------------------------------- | ----------------------------------- |
| point   | 16 bytes     | Point on a plane                      | (x,y)                               |
| line    | 32 bytes     | Infinite line (not fully implemented) | ((x1,y1),(x2,y2))                   |
| lseg    | 32 bytes     | Finite line segment                   | ((x1,y1),(x2,y2))                   |
| box     | 32 bytes     | Rectangular box                       | ((x1,y1),(x2,y2))                   |
| path    | 16+16n bytes | Closed path (similar to polygon)      | ((x1,y1),...)                       |
| path    | 16+16n bytes | Open path                             | [(x1,y1),...]                       |
| polygon | 40+16n       | Polygon (similar to closed path)      | ((x1,y1),...)                       |
| circle  | 24 bytes     | Circle                                | <(x,y),r> (center point and radius) |

### **Network Address Type**

PostgreSQL offers data types to store IPv4, IPv6, and MAC addresses. It is better to use these types instead of plain text types to store network addresses, because these types offer input error checking and specialized operators and functions.

| Name    | Storage Size  | Description                      |
| ------- | ------------- | -------------------------------- |
| cidr    | 7 or 19 bytes | IPv4 and IPv6 networks           |
| inet    | 7 or 19 bytes | IPv4 and IPv6 hosts and networks |
| macaddr | 6 bytes       | MAC addresses                    |

### **Bit String Type**

Bit String Types are used to store bit masks. They are either 0 or 1. There are two SQL bit types: **bit(n)** and **bit varying(n)**, where n is a positive integer.

### **Text Search Type**

This type supports full text search, which is the activity of searching through a collection of natural-language documents to locate those that best match a query. There are two Data Types for this −

| S. No.                                                                                                                               | Name & Description                                                               |
| ------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------- |
| 1                                                                                                                                    | tsvector                                                                         |
| This is a sorted list of distinct words that have been normalized to merge different variants of the same word, called as "lexemes". |
| 2                                                                                                                                    | tsquery                                                                          |
| This stores lexemes that are to be searched for, and combines them honoring the Boolean operators & (AND),                           | (OR), and ! (NOT). Parentheses can be used to enforce grouping of the operators. |

### **UUID Type**

A UUID (Universally Unique Identifiers) is written as a sequence of lower-case hexadecimal digits, in several groups separated by hyphens, specifically a group of eight digits, followed by three groups of four digits, followed by a group of 12 digits, for a total of 32 digits representing the 128 bits.

An example of a UUID is − 550e8400-e29b-41d4-a716-446655440000

### **XML Type**

The XML data type can be used to store XML data. For storing XML data, first you have to create XML values using the function xmlparse as follows −

```
XMLPARSE (DOCUMENT '<?xml version="1.0"?>
<tutorial>
<title>PostgreSQL Tutorial </title>
   <topics>...</topics>
</tutorial>')

XMLPARSE (CONTENT 'xyz<foo>bar</foo><bar>foo</bar>')
```

### **JSON Type**

The *json* data type can be used to store JSON (JavaScript Object Notation) data. Such data can also be stored as *text*, but the *json* data type has the advantage of checking that each stored value is a valid JSON value. There are also related support functions available, which can be used directly to handle JSON data type as follows.

| Example                                  | Example Result      |
| ---------------------------------------- | ------------------- |
| array_to_json('{{1,5},{99,100}}'::int[]) | [[1,5],[99,100]]    |
| row_to_json(row(1,'foo'))                | {"f1":1,"f2":"foo"} |

### **Array Type**

PostgreSQL gives the opportunity to define a column of a table as a variable length multidimensional array. Arrays of any built-in or user-defined base type, enum type, or composite type can be created.

```sql
CREATE TABLE monthly_savings (
   name text,
   saving_per_quarter integer[],
   scheme text[][]
);
```

### `Primary Keys` and `Foreign Keys`

- `Primary Key` : A primary key is column or a group of columns used to identify a row uniquely in a table.
  - Features: `unique` and `non-null`
- `Foreign Key` : A foreign key is a field or group fields in a table that uniquely identifies a row in another table.
  - A foreign key is defined in a table that references to the primary key of the other table.
  - The table that contains the foreign key is called referencing table or child table.
  - The table to which the foreign key references is called referenced table or parent table.
  - A table can have multiple foreign keys depending on its relationship with other tables.

### `Constraints`

- Constraints are the rules enforces on data column on table.
- These are used to prevent invalid data from being entered into the database.

### Column Constraints:

- `**NOT-NULL`: Ensures that a column cannot have NULL value.\*\*
- `**UNIQUE`: Ensures that all values in a column are different.\*\*
- `**Primary Key**`
- `**Foreign Key**`
- `CHECK`: Ensures that all values in a column satisfy certain constraints.
- `EXCLUSION`: The EXCLUDE constraint ensures that if any two rows are compared on the specified column(s) or expression(s) using the specified operator(s), not all of these comparisons will return TRUE.

### Table Constraints:

- `CHECK(condition)`
- `REFERENCES`
- `UNIQUE`(column_list)
- `Primary Key` (column_list)

### `CREATE` Table

- General Syntax

```sql
CREATE TABLE table_name(
column_name TYPE column_constraint,
column_name TYPE column_constraint
table_constraint table_constraint)
INHERITS exisiting_table_name
```

- Example

```sql
CREATE TABLE table_name(
column_id SERIAL PRIMARY KEY,
referenced_column_name DATATYPE(limit)
REFERENCES referenced_table_name(column_name)
)
```

```sql
CREATE TABLE table_name(
column_id SERIAL NOT NULL,
tag_id SERIAL NOT NULL,
referenced_column_name DATATYPE(limit)
REFERENCES referenced_table_name(column_name),
PRIMARY KEY(tag_id,column_id) # table constraints
)
```

### `INSERT`

```sql
INSERT INTO table_name(column1, column2,....)
VALUES
(value1,value2,...),
(value1,value2,...),....;

```

```sql
INSERT INTO table_name(column1, column2,....)
SELECT column1, column2, ...
FROM another_table_name
WHERE condition;
```

### `UPDATE`

```sql
UPDATE table_name
SET column1=value1,
		column2=value2,....
WHERE condition;
```

```sql
UPDATE tableA
SET original_col = tableB.new_col,
FROM tableB
WHERE tableA.id = tableB.id
RETURNING column1, column2,....;
```

### `DELETE`

```sql
DELETE FROM table_name
WHERE row_id=1;
```

```sql
DELETE FROM tableA
USING tableB
WHERE tableA.id = tableB.id;
```

### `ALTER`

refer this [https://www.postgresql.org/docs/current/sql-altertable.html](https://www.postgresql.org/docs/current/sql-altertable.html)

- Adding Column

```sql
ALTER TABLE table_name
ADD COLUMN new_column TYPE
```

- Deleting Column

```sql
ALTER TABLE table_name
DROP COLUMN column_name
```

- Alter Constraints

```sql
ALTER TABLE table_name
ALTER COLUMN column_name
SET DEFAULT value or DROP DEFAULT or SET NOT NULL or DROP NOT NULL
or ADD CONSTRAINT constraint_name
```

- Renaming Column

```sql
ALTER TABLE table_name
RENAME COLUMN old_column_name TO new_column_name
```

### `DROP`

```sql
# this doesn't remove any dependencies
ALTER TABLE table_name
DROP COLUMN IF EXISTS column_name

# to remove column with dependencies
ALTER TABLE table_name
DROP COLUMN column_name CASCADE
```

### `CHECK` Constraints

- The Check Constraint allows us to create more customized constraints that adhere to certain condition.

```sql
# Example:
CREATE TABLE example(
ex_id SERIAL PRIMARY KEY,
age SMALLINT NOT NULL CHECK(age>21),
parent_age SMALLINT NOT NULL CHECK(parent_age>age));

```

## Conditional Expressions and Procedures Introduction

### `CASE`

- This is If/Else of SQL.

CASE STATEMENT SYNTAX

```sql
CASE
	WHEN condtion1 THEN result1
	WHEN condtion2 THEN result2
	ELSE some_other_result
END

# Example
SELECT a,
CASE
	WHEN a=1 THEN 'one'
	WHEN a=2 THEN 'two'
	ELSE 'other'
END
FROM table_name;
```

CASE EXPRESSION SYNTAX

```sql
CASE expression
	WHEN value1 THEN result1
	WHEN value2 THEN result2
	ELSE some_other_result
END

# Example
SELECT a,
CASE a
	WHEN 1 THEN 'one'
	WHEN 2 THEN 'two'
	ELSE 'other'
END
FROM table_name;

"You can put CASE and END inside SUM() to count the values which satisfies
condition."
```

### COALESCE

- The COALESCE function accepts an unlimited number of arguments. It returns the first argument that is not null. If all arguments are null, the COALESCE function will return null.
  - COALESCE(arg1, arg2, arg3,….., argn)

```sql
# EXAMPLE:
SELECT item, (price - COALESCE(discount,0))
AS final FROM table;

#here the null values are replaced by 0 but not in the table only
#for this query.
```

### CAST (::)

- CAST operator let’s you convert from one data type into other.

```sql
SELECT CAST('5' AS INTEGER)
or
SELECT '5'::INTEGER

# EXAMPLE:
SELECT date::TIMESTAMP
FROM table_name
```

### NULLIF

- NULLIF(arg_1, arg_2)
  - if arg_1 = arg_2 then returns NULL
  - else returns arg_1

### VIEWS

- Views are virtual table which does not store data physically.

```sql
CREATE VIEW view_name AS
SELECT column_name_1, column_name_2
FROM tableA
INNER JOIN tableB
ON tableA.id = tableB.id
--
SELECT * from view_name
--
DROP VIEW IF EXISTS view_name
```
