# Deletions

Rows can be removed from a table using the *DELETE* command. 

Suppose you are not longer interested in the weather of Hayward. Then you can do the following to delete those rows from the table:

```sql
DELETE FROM weather WHERE city = 'Hayward';
```

```output
     city      | temp_lo | temp_hi | prcp |    date    
---------------+---------+---------+------+------------
 San Francisco |      46 |      50 | 0.25 | 1994-11-27
 San Francisco |      39 |      53 |    0 | 1994-11-29
(2 rows)

```

- `To remove all rows from the given table.

```sql
DELETE FROM tablename;
```