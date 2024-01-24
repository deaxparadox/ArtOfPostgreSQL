# PostgreSQL Column Alias

A column alias allows you to assign a column or an expression in the select list of a `SELECT` statement a temporary name. The column alias exits temporarily during the execution of the query.

The following illustrates the syntax of using a column alias:

```sql
SELECT column_name AS alias_name
FROM table_name;
```

The `AS` keyword is optional so you can omit it like this:

```sql
SELECT column_name alias_name
FROM table_name;
```

The following syntax illustrates how to set an aliaas for an expression in the `SELECT` clause:

```sql
SELECT expression AS alias_name
FROM table_name;
```

The main purpose of column alias is to make the headings of the output of a query more meaningful.

### 1) Assigning a column alias to a column example

The following query returns the first names and last names of all customers from the customers from the `customer` table:

```sql
SELECT 
   first_name, 
   last_name
FROM customer;
```

```
 first_name  |  last_name   
-------------+--------------
 Jared       | Ely
 Mary        | Smith
 Patricia    | Johnson
 Linda       | Williams
 Barbara     | Jones
 Elizabeth   | Brown
 Jennifer    | Davis
 Maria       | Miller
 Susan       | Wilson
 Margaret    | Moore
 Dorothy     | Taylor
 Lisa        | Anderson
 Nancy       | Thomas
 Karen       | Jackson
```


If you want to rename the `last_name` heading, you can assign it a new name using a column alias like this:

```sql
SELECT first_name, last_name as surname FROM customer;
```

This query assigned the `surname` as the alias of the `last_name` column:

```
 first_name  |   surname    
-------------+--------------
 Jared       | Ely
 Mary        | Smith
 Patricia    | Johnson
 Linda       | Williams
 Barbara     | Jones
 Elizabeth   | Brown
 Jennifer    | Davis
 Maria       | Miller
 Susan       | Wilson
 Margaret    | Moore
 Dorothy     | Taylor
 Lisa        | Anderson
 Nancy       | Thomas
 Karen       | Jackson
 Betty       | White
 Helen       | Harris
 Sandra      | Martin
 Donna       | Thompson
``` 

Or you can make it shorter by removing the `AS` keyword follows.

### 2) Assigning a column alias to an expression example

The following query returns the full names of all customers. It constructs the full name by concatenating the first name, space, and the last name:

```sql
SELECT 
   first_name || ' ' || last_name 
FROM 
   customer;
```

Note that in PostgreSQL, you use the `||` as the concatenating operator that concatenates one or more strings into a single string.

```sql
       full_name       
-----------------------
 Jared Ely
 Mary Smith
 Patricia Johnson
 Linda Williams
 Barbara Jones
 Elizabeth Brown
 Jennifer Davis
 Maria Miller

```

### 3) Column aliases that contain spaces

If a column alias contains one or more spaces, you need to surround it with double quotes like this:

```sql
column_name AS "column alias"
```

For example:

```sql
SELECT
    first_name || ' ' || last_name "full name"
FROM
    customer;
```


```sql
       full_name       
-----------------------
 Jared Ely
 Mary Smith
 Patricia Johnson
 Linda Williams
 Barbara Jones
 Elizabeth Brown
 Jennifer Davis
 Maria Miller

```