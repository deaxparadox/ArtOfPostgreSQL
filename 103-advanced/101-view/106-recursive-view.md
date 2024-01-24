# PostgreSQL Recursive View

- [<<< Updatable Views using the with CHECK OPTION Clause](105-updatable-views-using-the-with-check-option-clause.md) | [Indexes >>>](../102-indexes/README.md)

----------

### Introduction to the PostgreSQL recursive view

The `CREATE RECURSIVE VIEW` statement is syntax sugar for a standard recursive query.

The following illustrates the `CREATE RECURSIVE VIEW` syntax:

```sql
CREATE RECURSIVE VIEW view_name(columns) AS
SELECT columns;
```

First, specify the name of the view that you want to create in the `CREATE RECURSIVE VIEW` clause. You can add an optional schema-qualified to the name of the view.

Second, add the SELECT statement to the query data from base tables. The `SELECT` statement references the `view_name` to make the view recursive.

The statement above is equivalent to the following statement:


```sql
CREATE VIEW view_name 
AS
  WITH RECURSIVE cte_name (columns) AS (
    SELECT ...)
  SELECT columns FROM cte_name;
```

### Creating recursive view example

The following recursive query returns the employee and their managers up the the CEO level using a common table expression or CTE.

```sql
WITH RECURSIVE reporting_line AS (
	SELECT
		employee_id,
		full_name AS subordinates
	FROM
		employees
	WHERE
		manager_id IS NULL
	UNION ALL
		SELECT
			e.employee_id,
			(
				rl.subordinates || ' > ' || e.full_name
			) AS subordinates
		FROM
			employees e
		INNER JOIN reporting_line rl ON e.manager_id = rl.employee_id
) SELECT
	employee_id,
	subordinates
FROM
	reporting_line
ORDER BY
	employee_id;
```

```
 employee_id |                         subordinates
-------------+--------------------------------------------------------------
           1 | Michael North
           2 | Michael North > Megan Berry
           3 | Michael North > Sarah Berry
           4 | Michael North > Zoe Black
           5 | Michael North > Tim James
           6 | Michael North > Megan Berry > Bella Tucker
           7 | Michael North > Megan Berry > Ryan Metcalfe
           8 | Michael North > Megan Berry > Max Mills
           9 | Michael North > Megan Berry > Benjamin Glover
          10 | Michael North > Sarah Berry > Carolyn Henderson
          11 | Michael North > Sarah Berry > Nicola Kelly
          12 | Michael North > Sarah Berry > Alexandra Climo
          13 | Michael North > Sarah Berry > Dominic King
          14 | Michael North > Zoe Black > Leonard Gray
          15 | Michael North > Zoe Black > Eric Rampling
          16 | Michael North > Megan Berry > Ryan Metcalfe > Piers Paige
          17 | Michael North > Megan Berry > Ryan Metcalfe > Ryan Henderson
          18 | Michael North > Megan Berry > Max Mills > Frank Tucker
          19 | Michael North > Megan Berry > Max Mills > Nathan Ferguson
          20 | Michael North > Megan Berry > Max Mills > Kevin Rampling
(20 rows)
```

You can use the `CREATE RECURSIVE VIEW` statement to convert the query into a recursive view as follows:

```sql
CREATE RECURSIVE VIEW reporting_line (employee_id, subordinates) AS 
SELECT
	employee_id,
	full_name AS subordinates
FROM
	employees
WHERE
	manager_id IS NULL
UNION ALL
	SELECT
		e.employee_id,
		(
			rl.subordinates || ' > ' || e.full_name
		) AS subordinates
	FROM
		employees e
	INNER JOIN reporting_line rl ON e.manager_id = rl.employee_id;
```

To see the reporting line of the employee id 10, you query directly from the view:

```sql
SELECT
	subordinates
FROM
	reporting_line
WHERE
	employee_id = 10;

```

```
                  subordinates
-------------------------------------------------
 Michael North > Sarah Berry > Carolyn Henderson
(1 row)
```

----------

- [<<< Updatable Views using the with CHECK OPTION Clause](105-updatable-views-using-the-with-check-option-clause.md) | [Indexes >>>](../102-indexes/README.md)
