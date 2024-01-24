# PostgreSQL ORDER BY

### Introduction to PostgreSQL ORDER BY clause

When you query data from a table, the `SELECT` statement returns rows in an unspecified order. To sort the rows of the result set, you use the `ORDER BY` clause in the `SELECT` statement.

The `ORDER BY` clause allows you to sort rows returned by a `SELECT` clause in ascending or descending order based on a sort expression.

The folowing illustrates the syntax of the `ORDER BY` clause:

```sql
SELECT
 select_list
FROM
 table_name
ORDER BY
 sort_expression1 [ASC | DESC],
        ...
 sort_expressionN [ASC | DESC];
```

In this syntax:

- First, specify a sort expression, which can be a column or an expression, that you want to sort after the `ORDER BY` keywords. If you want to sort the result set based on multiple columns or expressions, you need to place a comma (`,`) between two columns or expressions to separate them.
- Second, you use the `ASC` option to sort rows in ascending order and the `DESC` option to sort rows in descending order. If you omit the `ASC` or `DESC` option, the `ORDER BY` uses `ASC` by default.

PostgreSQL evaluates the clauses in the `SELECT` statment in the following order: `FROM`, `SELECT`, and `ORDER BY`:

![order by 1](asset/order-by-1.png)

Due to the order of evaluation, if you have a column alias in the `SELECT` clause, you can use it in the `ORDER BY` clause.

### PostgreSQL ORDER BY examples

1) Using PostgreSQL ORDER BY clause to sort rows by one column

The following query uses the `ORDER BY` claues to sort customers by their first name in ascending order:

```sql
SELECT
 first_name,
 last_name
FROM
 customer
ORDER BY
 first_name ASC;
```

```sql
 first_name  |  last_name   
-------------+--------------
 Aaron       | Selby
 Adam        | Gooch
 Adrian      | Clary
 Agnes       | Bishop
 Alan        | Kahn
 Albert      | Crouse
 Alberto     | Henning
```

Since the `ASC` option is the default, you can omit it in the `ORDER BY` clause like this:

```sql
SELECT
 first_name,
 last_name
FROM
 customer
ORDER BY
 first_name;
```

### 2) Using PostgreSQL ORDER BY clause to sort rows by one column in descending order

The following statement selects the first name and last name from the `customer` table and sorts the rwos by values in the last name column in descending order:

```sql
SELECT
       first_name,
       last_name
FROM
       customer
ORDER BY
       last_name DESC;
```

```sql
 first_name  |  last_name   
-------------+--------------
 Zachary     | Hite
 Yvonne      | Watkins
 Yolanda     | Weaver
 Wilma       | Richards
 Willie      | Markham
 Willie      | Howell
```

### 3) Using PostgreSQL ORDER BY clause to sort rows by multiple columns

The following statement selects the first name and last name from the customer table and sorts the rows by the first name in ascending order and last name in descending order:

```sql
SELECT
 first_name,
 last_name
FROM
 customer
ORDER BY
 first_name ASC,
 last_name DESC;
```

```sql
 first_name  |  last_name   
-------------+--------------
 Aaron       | Selby
 Adam        | Gooch
 Adrian      | Clary
 Agnes       | Bishop
 Alan        | Kahn
 Albert      | Crouse
 Alberto     | Henning
```

In this example, the ORDER BY clause sorts rows by values in the first name column first. And then it sorts the sorted rows by values in the last name column.

```sql
dvdrental=# select first_name, last_name from customer where first_name = 'Kelly' order by first_name a
sc, last_name desc;

 first_name | last_name 
------------+-----------
 Kelly      | Torres
 Kelly      | Knott
(2 rows)
```

As you can see clearly from the output, two customers with the same first name `Kelly` have the last name sorted in descending order.

### 4) Using PostgreSQL ORDER BY clause to sort rows by expressions

The `LENGTH()` function accepts a string and returns the length of that string.

The following statement selects the first names and their lengths. It sorts the rows by the lengths of the first names:

```sql
SELECT 
	first_name,
	LENGTH(first_name) len
FROM
	customer
ORDER BY 
	len DESC;

-- or 
-- dvdrental=# select first_name, length(first_name) as len from customer order by len desc;
```	

```sql
 first_name  | len 
-------------+-----
 Christopher |  11
 Jacqueline  |  10
 Constance   |   9
 Katherine   |   9
 Nathaniel   |   9
 Catherine   |   9
 Christian   |   9
 Christine   |   9
 Charlotte   |   9
```

Because the `ORDER BY` clause is evaluated after the `SELECT` clause, the column alias `len` is available and can be used in the `ORDER BY` clause.

### PostgreSQL ORDER BY clause and NULL

In the database world, `NULL` is a marker that indicates the missing data or the data is unknown at the time of recording.

When you sort rows that contains `NULL`,  you can specify the order of `NULL` with other non-null values by using the `NULLS FIRST` or `NULLS LAST` option of the `ORDER BY` clause:


```sql
ORDER BY sort_expresssion [ASC | DESC] [NULLS FIRST | NULLS LAST]
```

The `NULLS FIRST` option places `NULL` before other non-null values and the `NULL LAST` option places `NULL` after other non-null values.


Let's create a table for the demonstration:

```sql
-- create a new table
CREATE TABLE sort_demo(
	num INT
);

-- insert some data
INSERT INTO sort_demo(num)
VALUES(1),(2),(3),(null);
```

The following query returns data from the `sort_demo` table:

```sql
SELECT num
FROM sort_demo
ORDER BY num;
```

```sql
 num 
-----
   1
   2
   3
    
(4 rows)
```

So if you use the `ASC` option, the `ORDER BY` clause uses the `NULLS LAST` option by default. Therefore, the following query returns the same result:

```sql
SELECT num
FROM sort_demo
ORDER BY num NULLS LAST;
```

```sql
dvdrental=# select num from sort_demo order by num nulls first;
 num 
-----
    
   1
   2
   3
(4 rows)
```


The following statement sorts values in the `num` column of the `sort_demo` table in descending order:

```sql
SELECT num
FROM sort_demo
ORDER BY num DESC;
```

```sql
dvdrental=# select num from sort_demo order by num desc;
 num 
-----
    
   3
   2
   1
(4 rows)
```