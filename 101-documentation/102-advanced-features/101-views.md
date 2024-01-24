# Views

Suppose the combined listing of weather records and city location is of particular interest to your application, but you do not want to type the query each time you need it.

You can create a *view* over the query, which gives a name to the query that you can query to like an ordinary table:

```sql
# CREATE VIEW myview AS SELECT name, temp_lo, temp_hi, prcp, date, location FROM weather, cities WHERE city = name;

# select  * from myview;
```

- Views allow you to encapsulate the details of the structure of your tables, which might changes as your application evolves behind consistent interfaces.