### Querying a Table

To retrieve data from a table, the table is *queried*. An SQL `SELECT` statement is used to do this. This statement is divided into a select list (the part that lists the columns to be retruned), a table lsit (the part that lists tables from which to retreive the data), and an optinal qualification (the part that specifies any restrictions).

To retrieve all the rows from the table:

```sql
test=# SELECT * FROM weather;
     city      | temp_lo | temp_hi | prcp |    date    
---------------+---------+---------+------+------------
 San Francisco |      46 |      50 | 0.25 | 1994-11-27
 San Francisco |      43 |      57 |    0 | 1994-11-29
 Hayward       |      37 |      54 |      | 1994-11-29
(3 rows) 
```

Here `*` is a shorthand for "all columns".

So the same result would be had with

```sql
test=# SELECT city, temp_lo, temp_hi, prcp, date FROM weather;
     city      | temp_lo | temp_hi | prcp |    date    
---------------+---------+---------+------+------------
 San Francisco |      46 |      50 | 0.25 | 1994-11-27
 San Francisco |      43 |      57 |    0 | 1994-11-29
 Hayward       |      37 |      54 |      | 1994-11-29
(3 rows)

```

You can write expressions, not just simple column references, in the select list. For example, you can calculate the average temperature:

```sql                                                     
test=# SELECT city, (temp_lo + temp_hi)/2 AS average_temperature, date FROM weather;
     city      | average_temperature |    date    
---------------+---------------------+------------
 San Francisco |                  48 | 1994-11-27
 San Francisco |                  50 | 1994-11-29
 Hayward       |                  45 | 1994-11-29
(3 rows)
```

`AS` clause is used to relabel the output column

#### WHERE clause

A query can be "qualified" by adding a `WHERE` clause that specifies which rows are wanted. The `WHERE` clause contains a Boolean (truth value) expression, and only rows which the Boolean expression is true are returned. The usual Boolean operators (AND, OR, and NOT) are allowed in the qualification.

For example, the following retrieves the weather of San Francisco on rainy days:



```sql
test=# SELECT * FROM weather WHERE city = 'San Francisco' AND prcp > 0.0;
     city      | temp_lo | temp_hi | prcp |    date    
---------------+---------+---------+------+------------
 San Francisco |      46 |      50 | 0.25 | 1994-11-27
(1 row)
```

## `ORDER BY` Clause

You can request that the results of a query be returned in sorted order:

```sql
test=# SELECT * FROM weather ORDER BY city;
     city      | temp_lo | temp_hi | prcp |    date    
---------------+---------+---------+------+------------
 Hayward       |      37 |      54 |      | 1994-11-29
 San Francisco |      46 |      50 | 0.25 | 1994-11-27
 San Francisco |      43 |      57 |    0 | 1994-11-29
(3 rows)
```


## `DISTINCT` clause

You can request that duplicate rows be removed from the result of a query:

```sql
test=# SELECT DISTINCT city FROM weather;
     city      
---------------
 Hayward
 San Francisco
(2 rows)

```

You can combine `ORDER BY` and `DISTINCT` together;

```sql
test=# SELECT DISTINCT * FROM weather ORDER BY temp_lo;
  city  | temp_lo | temp_hi | prcp |    date    
--------+---------+---------+------+------------
 Dehli  |      30 |      43 | 0.25 | 1994-11-27
 Kanpur |      30 |      52 | 0.25 | 1994-11-27
 Kanpur |      42 |      50 | 0.25 | 1994-11-27
 Kanpur |      42 |      53 | 0.25 | 1994-11-27
 Dehli  |      46 |      50 | 0.25 | 1994-11-27
 Kanpur |      55 |      31 | 0.13 | 1994-12-24
(6 rows)

test=# 
```