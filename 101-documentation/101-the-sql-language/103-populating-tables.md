# Populating a Table with Rows

## Inserting with columns order

The `INSERT` statement is used to populate a table with rows:

Earlier we have learn to created database. We will continue with *weather* database we created earliar.

```sql
INSERT INTO weather VALUES ('San Francisco', 46, 50, 0.25, '1994-11-27');
```
Constants that are not simple numeric values usually must be surrounded by single quotes (`'`), as in the example.

The *point* type requires a coordinate pair as input:

```sql
INSERT INTO cities VALUES ('San Franscisco', '(-192.0, 53.0)');
```

# Inserting with columns name

The syntax used so far requires you to remember the order of the columns. An alternative syntax allows you to list the columns explicitly:



```sql
INSERT INTO weather (city, temp_lo, temp_hi, prcp, date)
	VALUES ('San Francisco', 43, 57, 0.0, '1994-11-29');
```

You can list the columns in a different order if you wish or even omit some columns e.g., if the percipitation is unknown:

```sql
INSERT INTO weather (date, city, temp_hi, temp_lo) VALUES ('1994-11-29', 'Hayward', 54, 37);
```