# Joins Between tables


## `INNER` Joins

Before moving forward let's create another table for join:

```sql
test=# INSERT INTO cities (name, location) VALUES ('Dehli', '(-194.0, 53.0)');
INSERT 0 1
test=# select * from cities;
 name  | location  
-------+-----------
 Dehli | (-194,53)
(1 row)

test=# 
```


Queries that access multipls tables (or multiple instances of the same table) at one time are called *join* queries. They combine row of one table with row of second table, with an expression specifying which rows are to be paired.

For example, to return all the weather records together with the location of the associated city, the database needs to compare the *city* column of each row of the *weather* table with the name column of all rows in the cities table, and select the pairs of rows where these values match. This would be accomlished by the following query:


```sql
test=# SELECT * FROM weather JOIN cities ON city = name;
 city  | temp_lo | temp_hi | prcp |    date    | name  | location  
-------+---------+---------+------+------------+-------+-----------
 Dehli |      30 |      43 | 0.25 | 1994-11-27 | Dehli | (-194,53)
 Dehli |      46 |      50 | 0.25 | 1994-11-27 | Dehli | (-194,53)
(2 rows)

test=# 
```

These is no result row for the city *Kanpur*. This is because there is no matching entry in the *cities* table for *Kanpur*, so the join ignores the unmatched rows in the weather table. 


If there were duplicate columns names in the two tables you'd need to *qualify* the column name to show which on you meant, as in:

```sql
test=# SELECT weather.city, weather.temp_lo, weather.temp_hi, cities.name, cities.location FROM weather JOIN cities ON weather.city = cities.name;
 city  | temp_lo | temp_hi | name  | location  
-------+---------+---------+-------+-----------
 Dehli |      30 |      43 | Dehli | (-194,53)
 Dehli |      30 |      43 | Dehli | (-194,53)
 Dehli |      46 |      50 | Dehli | (-194,53)
 Dehli |      46 |      50 | Dehli | (-194,53)
(4 rows)

test=# 
```

## `OUTER` Join

Now we will figure out how we can get the Hayward records back in.

What we want the query to do is to scan the *weather* table and for each row to findthe matching *cities* row(s). If no matching row is found we want some "empty" values to be substituted for the cities table's columns. This kind of query is called *outer join*. (The joins we have seen so far are *inner joins*.)

### LEFT OUTER JOIN

```sql
SELECT * FROM weather LEFT OUTER JOIN cities on weather.city = cities.name;
```

```output
     city      | temp_lo | temp_hi | prcp |    date    |     name      | location  
---------------+---------+---------+------+------------+---------------+-----------
 San Francisco |      46 |      50 | 0.25 | 1994-11-27 | San Francisco | (-192,53)
 San Francisco |      43 |      57 |    0 | 1994-11-29 | San Francisco | (-192,53)
 Hayward       |      37 |      54 |      | 1994-11-29 |               | 
(3 rows)

```

This query is called a *left outer join* because the table mentioned on the left of the join operator will have each of its rows in the output at least once, whereas the table on the right will only have those rows output that match some row of the the left table. When outputting a left-table row for which there is no right-table match, empty (null) values are substutited for the right-table columns.

### SELF JOIN

We can also join a table against itself. This is called a *self join*.

Example: Suppose we wish to find all the weather records that are in the temparature range of other weather records. So we need to compare the *temp_lo* and *temp_hi* columns of each weather row to the *temp_lo* and *temp_hi* columns of all other weather rows.

```sql

SELECT 
w1.city, 
w1.temp_lo AS low, 
w1.temp_hi AS high, 
w2.city, 
w2.temp_lo AS low, 
w2.temp_hi AS high 
FROM weather AS w1 JOIN weather AS w2 
ON w1.temp_lo < w2.temp_lo 
AND w1.temp_hi > w2.temp_hi;
```

```output
     city      | low | high |     city      | low | high 
---------------+-----+------+---------------+-----+------
 San Francisco |  43 |   57 | San Francisco |  46 |   50
 Hayward       |  37 |   54 | San Francisco |  46 |   50
(2 rows)
```


