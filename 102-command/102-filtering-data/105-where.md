# PostgreSQL WHERE clause overview

The syntax of the PostgreSQL `WHERE` clause is as follows:

```sql
SELECT select_list
FROM table_name
WHERE condition
ORDER BY sort_expression
```

The `WHERE` clause appears right after the `FROM` clause of the `SELECT` statement.  The `WHERE` clause uses the condition to filter the rows returned from the `SELECT` clause.

The `condition` must evaluate to true, false, or unknown. It can be a boolean expression or a combination of boolean expressions using the `AND` and `OR` operators.

The query returns only rows that satisfy the `condition` in the `WHERE` clause. In other words, only rows that cause the `condition` evaluates to true will be included in the result set.

PostgreSQL evaluates the `WHERE` clause after the `FROM` clause and before the `SELECT` and `ORDER BY` clause:

![101 where](asset/101-where.png)

If you use column aliases in the `SELECT` clause, you cannot use them in the `WHERE` clause.

Besides the `SELECT` statement, you can use the `WHERE` clause in the `UPDATE` and `DELETE` statement to specify rows to be updated or deleted.

To form the condition in the `WHERE` clause, you use comparison and logical operators:

<table><thead><tr><th scope="col">Operator</th><th scope="col">Description</th></tr></thead><tbody><tr><td>=</td><td>Equal</td></tr><tr><td>&gt;</td><td>Greater than</td></tr><tr><td>&lt;</td><td>Less than</td></tr><tr><td>&gt;=</td><td>Greater than or equal</td></tr><tr><td>&lt;=</td><td>Less than or equal</td></tr><tr><td>&lt;&gt; or !=</td><td>Not equal</td></tr><tr><td>AND</td><td>Logical operator AND</td></tr><tr><td>OR</td><td>Logical operator OR</td></tr><tr><td><a href="https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-in/">IN</a></td><td>Return true if a value matches any value in a list</td></tr><tr><td><a href="https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-between/">BETWEEN</a></td><td>Return true if a value is between a range of values</td></tr><tr><td><a href="https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-like/">LIKE</a></td><td>Return true if a value matches a pattern</td></tr><tr><td><a href="https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-is-null/">IS NULL</a></td><td>Return true if a value is NULL</td></tr><tr><td> NOT</td><td>Negate the result of other operators</td></tr></tbody></table>

<h2>PostgreSQL WHERE clause examples</h2>

<p>
    Let’s practice with some examples of using the <mark>WHERE</mark> clause. We will use the <mark>customer</mark> table from the sample database for demonstration.
</p>

<img src="asset/102-where.png" alt="">


<h3>1) Using Where clause with the equal (=) operator example</h3>

<div>
    <p>
        The following statement uses the <mark>WHERE</mark> clause customers whose first names are `Jamie`:
    </p>
    
    ```sql
    SELECT
        last_name,
        first_name
    FROM
        customer
    WHERE
        first_name = 'Jamie';
    ```
    
    <img src="asset/103-where.png" alt="">
</div>

<h3>2) Using WHERE clause with the AND operator example</h3>

<p>
    The following example finds customer whose first name and last name are <mark>Jamie</mark> and <mark>rice</mark> by using the <mark>AND</mark> logical operator to combine two Boolean expressions:
</p>

```sql
SELECT
	last_name,
	first_name
FROM
	customer
WHERE
	first_name = 'Jamie' AND 
        last_name = 'Rice';
```

<img src="asset/104-where.png" alt="">

<h3>3) Using WHERE clause with the IN operator example</h3>

<p>
    If you want to match a string with any string in a list, you can use the <mark>IN</mark> operator.
</p>

<p>
    For example, the following statement returns customers whose first name is <mark>Ann</mark>, or <mark>Anne</mark>, or <mark>Annie</mark>: 
</p>

<img src="asset/105-where.png" alt="">

<h3>5) Using the WHERE clause with the LIKE operator example</h3>

<p>
    To find a string that matches a specified pattern, you use the <mark>LIKE</mark> operator. The following example returns all customers whose first names start with the string <mark>Ann</mark>:
</p>


```sql
SELECT
	first_name,
	last_name
FROM
	customer
WHERE 
	first_name LIKE 'Ann%'
```

<img src="asset/106-where.png" alt="">

The <mark>%</mark> is called a wildcard that matches any string. The <mark>'Ann%'</mark> pattern matches any string that starts with <mark>'Ann'</mark>.


<h3>6) Using the WHERE clause with the BETWEEN operator example</h3>

<p>
    The following example finds customers whose first names start with the letter <mark>A</mark> and contains 3 to 5 characters by using the `BETWEEN operator.
</p>

<p>
    The <mark>BETWEEN</mark> operator returns true if a value is in a range of values.
</p>

```sql
SELECT
	first_name,
	LENGTH(first_name) name_length
FROM
	customer
WHERE 
	first_name LIKE 'A%' AND
	LENGTH(first_name) BETWEEN 3 AND 5
ORDER BY
	name_length;
```

```
first_name | name_length 
------------+-------------
 Amy        |           3
 Ann        |           3
 Ana        |           3
 Andy       |           4
 Anna       |           4
 Anne       |           4
 Alma       |           4
 Adam       |           4
 Alan       |           4
 Alex       |           4
 Angel      |           5
 Agnes      |           5
```

<p>
    In this example, we used the `LENGTH()` function gets the number of characters of an input string.
</p>

<h3>7) Using the WHERE clause with the not equal operator (&lt;&gt;) example</h3>

<p>
    This example finds customers whose first names start with `Bra` and last names are not `Motley`:
</p>


```sql
SELECT 
	first_name, 
	last_name
FROM 
	customer 
WHERE 
	first_name LIKE 'Bra%' AND 
	last_name <> 'Motley';
```

```
first_name | last_name 
------------+-----------
 Brandy     | Graves
 Brandon    | Huey
 Brad       | Mccurdy
(3 rows)
```

<p>
    Note that you can use the <mark>!=</mark> operator and <mark>&lt;&gt;</mark> operator interchangeably because they are equivalent.
</p>