# Aggregate Functions

An aggregate function computes a single result from multple input rows. For example, there are aggregates to compute the *count*, *sum*, *avg (average)* , *max (maximum)* and *min (minimum)* over a set of rows.

we can find the highest low-temperature reading anywhere with:

```sql
SELECT max(temp_lo) FROM weather;
```

```output
 max 
-----
  46
(1 row)
```

`Aggregate function cannot be used in the WHERE clause`. (This retriction exits because the WHERE clause determines which rows will be included in the aggregate calculation so obviously it has to be evaluated before aggregate functions are computed.)

- `subquery`: using aggregate with *WHERE* clause

```sql
SELECT *   
FROM weather
WHERE temp_lo = (SELECT max(temp_lo) FROM weather);
```

```output
     city      | temp_lo | temp_hi | prcp |    date    
---------------+---------+---------+------+------------
 San Francisco |      46 |      50 | 0.25 | 1994-11-27
(1 row)

```

- `Aggregates` with `GROUP BY` clauses. We can get the maximum low temperature observed in each city with, which gives us one output row per city.

```sql
SELECT city, max(temp_lo)
FROM weather
GROUP BY city;
```

```output
     city      | max 
---------------+-----
 Hayward       |  37
 San Francisco |  46
(2 rows)
```


- Each aggregate result is computed over the table rows matching that city. We can filter these grouped rows using *HAVING* and the output count using *FILTER*:

```sql
SELECT city, max(temp_lo), count(*)
FILTER (WHERE temp_lo < 30)
FROM weather
GROUP BY city
HAVING max(temp_lo) < 40;
```

```output
  city   | max | count 
---------+-----+-------
 Hayward |  37 |     0
(1 row)
```

which gives us the same results for only the cities that have all *temp_lo* values below 40.

- if we only care about cities whose name begin with *S*, we might do:

```sql
SELECT city, max(temp_lo), count(*) 
FILTER (WHERE temp_lo < 30) 
FROM weather
WHERE city LIKE '%S'
GROUP BY city
HAVING max(temp_lo) < 40;
```

### difference between WHERE and HAVING

#### WHERE 

- *WHERE* selects input rows before groups and aggregates are computed (this, it controls which rwos go into the aggregate computation), 


#### HAVING 

- whereare *HAVING* selects group rows after groups and aggregates are computed.
- *HAVING* clause always contains aggregate functions.