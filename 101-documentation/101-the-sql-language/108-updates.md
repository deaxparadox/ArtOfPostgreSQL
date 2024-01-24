# Updates 

You can update existing rows using the *UPDATE* command.

Support you discover the temperature readings are all off by 2 degrees after November 28.

```sql
UPDATE weather
SET temp_hi = temp_hi - 2, temp_lo = temp_lo - 2
WHERE date > '1994-11-28';
UPDATE 2
```

```sql
SELECT * FROM weather;
```

```output
     city      | temp_lo | temp_hi | prcp |    date    
---------------+---------+---------+------+------------
 San Francisco |      46 |      50 | 0.25 | 1994-11-27
 San Francisco |      39 |      53 |    0 | 1994-11-29
 Hayward       |      33 |      50 |      | 1994-11-29
(3 rows)

```