# PostgreSQL Materialized Views

- [<<< Creating Updatable Views](103-creating-updatable-views.md) | [Updatable Views Using the WITH CHECK OPTION Clause >>>](105-updatable-views-using-the-with-check-option-clause.md)

----------


Views are virtual tables that represent data of the underlying tables. Simple view can be also updatable.

PostgreSQL extends the view concept to the next level that allows views to store data physically. And these views are called materialized views.

Materialized views cache the result of a complex and expensive query and allow you to referesh this result periodically.

The materialized views are usefull in many cases that require fast data access therefore they are often used in data warehouses and business intelligence applications.

### Creating materialized views

To create a materiazed view, you use the `CREATE MATERIALIZED VIEW` statement follows:

```sql
CREATE MATERIALIZED VIEW view_name
AS
query
WITH [NO] DATA;
```

- First, specify the `view_name` after the `CREATE MATERIALIZED VIEW` clause
- Second, add the query that gets data from the underlying tables after the `AS` keyword.
- Third, if you want to load data into the materialized view at the creation time, use the `WITH DATA` option; otherwise, you use `WITH NO DATA`. In case you use `WITH NO DATA`, the view flagged as unreadable. It means that you cannot query data from the view untill you load data into it.

### Refereshing data for materialized views

To lead data into a mateiralized view, you use the `REFERESH MATERIALIZED VIEW` statement as show below:

```
REFRESH MATERIALIZED VIEW view_name;
```

When you referesh data for a materialized view, PostgreSQL locks the entire table therefore you cannot data against it. To avoid this, oyu can use the `CONCURRENTLY` option.

```
REFRESH MATERIALIZED VIEW CONCURRENTLY view_name;
```

With `CONCURRENTLY` option, PostgreSQL, creates a temporary updated version of the materialized view, compares two versions, and preforms INSERT and UPDATE only differenes.

You can query agagint a materialized view while it is being updated. One requirement for using `CONCURRENTLY` option is that the mateialized view must have a `UNIQUE` index.

#### Removing materialized views

Removing a materialized view is pretty straightforward as we hae done for tables or views. This is done using the following statement:

```
DROP MATERIALIZED VIEW view_name;
```

### PostgreSQL materialized views example

The following statement creates a materialized view name `rental_by_category`:

```sql
CREATE MATERIALIZED VIEW rental_by_category
AS
 SELECT c.name AS category,
    sum(p.amount) AS total_sales
   FROM (((((payment p
     JOIN rental r ON ((p.rental_id = r.rental_id)))
     JOIN inventory i ON ((r.inventory_id = i.inventory_id)))
     JOIN film f ON ((i.film_id = f.film_id)))
     JOIN film_category fc ON ((f.film_id = fc.film_id)))
     JOIN category c ON ((fc.category_id = c.category_id)))
  GROUP BY c.name
  ORDER BY sum(p.amount) DESC
WITH NO DATA;
```

Because of the `WITH NO DATA` option, you cannot query data from the view. If you try to do so, you'll get an error messages as follows:

```sql
SELECT
	*
FROM
	rental_by_category;
```

```
[Err] ERROR: materialized view "rental_by_category" has not been populated
HINT: Use the REFRESH MATERIALIZED VIEW command.
```

PostgreSQL is helpful to give you a hint to ask for loading data into the view. Let's do it by executing the following statement:

```
REFRESH MATERIALIZED VIEW rental_by_category;
```

Now, if you query data again, you will get the result as expected:


```
  category   | total_sales 
-------------+-------------
 Sports      |     4892.19
 Sci-Fi      |     4336.01
 Animation   |     4245.31
 Drama       |     4118.46
 Comedy      |     4002.48
 New         |     3966.38
 Action      |     3951.84
 Foreign     |     3934.47
 Games       |     3922.18
 Family      |     3830.15
 Documentary |     3749.65
 Horror      |     3401.27
 Classics    |     3353.38
 Children    |     3309.39
 Travel      |     3227.36
 Music       |     3071.52
```

For now on, you can refresh the data in the `rental_by_category` view using the `REFRESH MATERIALIZED VIEW` statement.

However, to refresh it with  `CONCURRENTLY` option, you need to create a `UNIQUE` index for the view first.

```sql
CREATE UNIQUE INDEX rental_category ON rental_by_category (category);
```

Let's refresh data concurrently for the `rental_by_category` view.

```
REFRESH MATERIALIZED VIEW CONCURRENTLY rental_by_category;
```

----------


- [<<< Creating Updatable Views](103-creating-updatable-views.md) | [Updatable Views Using the WITH CHECK OPTION Clause >>>](105-updatable-views-using-the-with-check-option-clause.md)
