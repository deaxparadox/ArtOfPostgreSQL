# PostgreSQL SELECT

We are going to learn how to use the **PostgreSQL SELECT** statemetn to query data from a table.

One of the most common tasks, when you work with the database, is to query data from tables by using the `SELECT` statement.

The `SELECT` statement is one of the most complex statements in PostgreSQL. It has many clauses that you can use to form a flexible query.

Because of its complexity, we will break it down into many shorter and easy-to-understand tutorials so that you can learn about each clause faster.

The `SELECT` statement has the following clauses:

- Select distinct rows using `DISTINCT` operator.
- Sort rows using `ORDER` BY clause.
- Filter rows using `WHERE` clause.
- Select a subset of rows from a table using `LIMIT` or `FETCH` clause.
- Group rows into groups using `GROUP BY` clause.
- Filter groups using `HAVING` clause.
- Join with other tables using joins such as `INNER JOIN`, `LEFT JOIN`, `FULL OUTER JOIN`, `CROSS JOIN` clauses.
- Perform set operations using `UNION`, `INTERSECT`, and `EXCEPT`.

For now we are going to focus on the `SELECT` and `FROM` clauses.


## PostgreSQL SELECT statement syntax


```sql
SELECT
   select_list
FROM
   table_name;
```

Let’s examine the `SELECT` statement in more detail:

- First, specify a select list that can be a column or a list of columns in a table from which you want to retrieve data. If you specify a list of columns, you need to place a comma (`,`) between two columns to separate them. If you want to select data from all the columns of the table, you can use an asterisk (`*`) shorthand instead of specifying all the column names. The select list may also contain expressions or literal values.

- Second, specify the name of the table from which you want to query data after the `FROM` keyword.

The `FROM` clause is optional. If you do not query data from any table you can omit the the `FROM` cluase in the `SELECT` statement.


PostgreSQL evaluates the `FROM` clause before the `SELECT` clause in the `SELECT` statement:

![select 1](asset/select-1.png)


## PostgreSQL SELECT examples

Let’s take a look at some examples of using PostgreSQL `SELECT` statement.

We will use the following `customer` table in the sample database for the demonstration.

![select 2 customer table](asset/select-2-customer-table.png)


### 1. Using PostgreSQL SELECT statement to query data from one column example.

This example uses the `SELECT` statement to find the first names of all customers from the `customer` table:


```sql
dvdrental=# select first_name from customer;
```

```sql
 first_name  
-------------
 Jared
 Mary
 Patricia
 Linda
 Barbara
 Elizabeth
 Jennifer
 Maria
 Susan
 Margaret
 Dorothy
 Lisa
 Nancy
 ...
```

Notice that we added a semicolon (`;`) at the end of the `SELECT` statement. The semicolon is not a part of the SQL statement. It is used to signal PostgreSQL the end of an SQL statement. The semicolon is also used to separate two SQL statements.


### 2. Using PostgreSQl Select statement to query data from multiple columns example

Suppose you just want to know the first name, last name and email of customers, you can specify these column names in the `SELECT` clause as shown in the following query:

```sql
SELECT
   first_name,
   last_name,
   email
FROM
   customer;
```

```sql
 first_name  |  last_name   |                  email                   
-------------+--------------+------------------------------------------
 Jared       | Ely          | jared.ely@sakilacustomer.org
 Mary        | Smith        | mary.smith@sakilacustomer.org
 Patricia    | Johnson      | patricia.johnson@sakilacustomer.org
 Linda       | Williams     | linda.williams@sakilacustomer.org
 Barbara     | Jones        | barbara.jones@sakilacustomer.org
 Elizabeth   | Brown        | elizabeth.brown@sakilacustomer.org
 Jennifer    | Davis        | jennifer.davis@sakilacustomer.org
 Maria       | Miller       | maria.miller@sakilacustomer.org
 Susan       | Wilson       | susan.wilson@sakilacustomer.org
 Margaret    | Moore        | margaret.moore@sakilacustomer.org
 Dorothy     | Taylor       | dorothy.taylor@sakilacustomer.org
 Lisa        | Anderson     | lisa.anderson@sakilacustomer.org
 Nancy       | Thomas       | nancy.thomas@sakilacustomer.org
 Karen       | Jackson      | karen.jackson@sakilacustomer.org
 Betty       | White        | betty.white@sakilacustomer.org
 Helen       | Harris       | helen.harris@sakilacustomer.org
 Sandra      | Martin       | sandra.martin@sakilacustomer.org
 ...
```

### 3. Using PostgreSQl SELECT statement to query data from all columns of a table example.


The following query uses the `SELECT` statement to select data from all columns of the `customer` table:

```sql
SELECT * FROM customer;
```

```sql
 customer_id | store_id | first_name  |  last_name   |                  email                   | address_id | activebool | create_date |       last_update       | active 
-------------+----------+-------------+--------------+------------------------------------------+------------+------------+-------------+-------------------------+--------
         524 |        1 | Jared       | Ely          | jared.ely@sakilacustomer.org             |        530 | t          | 2006-02-14  | 2013-05-26 14:49:45.738 |      1
           1 |        1 | Mary        | Smith        | mary.smith@sakilacustomer.org            |          5 | t          | 2006-02-14  | 2013-05-26 14:49:45.738 |      1
           2 |        1 | Patricia    | Johnson      | patricia.johnson@sakilacustomer.org      |          6 | t          | 2006-02-14  | 2013-05-26 14:49:45.738 |      1
           3 |        1 | Linda       | Williams     | linda.williams@sakilacustomer.org        |          7 | t          | 2006-02-14  | 2013-05-26 14:49:45.738 |      1
           4 |        2 | Barbara     | Jones        | barbara.jones@sakilacustomer.org         |          8 | t          | 2006-02-14  | 2013-05-26 14:49:45.738 |      1
           5 |        1 | Elizabeth   | Brown        | elizabeth.brown@sakilacustomer.org       |          9 | t          | 2006-02-14  | 2013-05-26 14:49:45.738 |      1
           6 |        2 | Jennifer    | Davis        | jennifer.davis@sakilacustomer.org        |         10 | t          | 2006-02-14  | 2013-05-26 14:49:45.738 |      1
           7 |        1 | Maria       | Miller       | maria.miller@sakilacustomer.org          |         11 | t          | 2006-02-14  | 2013-05-26 14:49:45.738 |      1
           8 |        2 | Susan       | Wilson       | susan.wilson@sakilacustomer.org          |         12 | t          | 2006-02-14  | 2013-05-26 14:49:45.738 |      1
           9 |        2 | Margaret    | Moore        | margaret.moore@sakilacustomer.org        |         13 | t          | 2006-02-14  | 2013-05-26 14:49:45.738 |      1
          10 |        1 | Dorothy     | Taylor       | dorothy.taylor@sakilacustomer.org        |         14 | t          | 2006-02-14  | 2013-05-26 14:49:45.738 |      1
          11 |        2 | Lisa        | Anderson     | lisa.anderson@sakilacustomer.org         |         15 | t          | 2006-02-14  | 2013-05-26 14:49:45.738 |      1
          12 |        1 | Nancy       | Thomas       | nancy.thomas@sakilacustomer.org          |         16 | t          | 2006-02-14  | 2013-05-26 14:49:45.738 |      1
```


In this example, we used an asterisk (`*`) in the `SELECT` clause, which is a shorthand for all columns. Instead of listing all columns in the SELECT clause, we just used the asterisk (`*`) to save some typing.

However, it is not a good practice to use the asterisk (`*`) in the SELECT statement when you embed SQL statements in the application code like Python, Java, Node.js, or PHP due to the following reasons:

1. Database performance. Suppose you have a table with many columns and a lot of data, the `SELECT` statement with the asterisk (`*`) shorthand will select data from all the columns of the table, which may not be necessary to the application.
2. Application performance. Retrieving unnecessary data from the database increases the traffic between the database server and application server. In consequence, your applications may be slower to respond and less scalable.

Because of these reasons, it is a good practice to explicitly specify the column names in the `SELECT` clause whenever possible to get only necessary data from the database.


And you should only use the asterisk (`*`) shorthand for the ad-hoc queries that examine data from the database.




### 4. Using PostgreSQL SELECT statement with expression example

The following example uses the `SELECT` statement to return full names and emails of all customers:

```sql
SELECT 
   first_name || ' ' || last_name,
   email
FROM 
   customer;
```

```sql
       ?column?        |                  email                   
-----------------------+------------------------------------------
 Jared Ely             | jared.ely@sakilacustomer.org
 Mary Smith            | mary.smith@sakilacustomer.org
 Patricia Johnson      | patricia.johnson@sakilacustomer.org
 Linda Williams        | linda.williams@sakilacustomer.org
 Barbara Jones         | barbara.jones@sakilacustomer.org
 Elizabeth Brown       | elizabeth.brown@sakilacustomer.org
 Jennifer Davis        | jennifer.davis@sakilacustomer.org
 Maria Miller          | maria.miller@sakilacustomer.org
 Susan Wilson          | susan.wilson@sakilacustomer.org
 Margaret Moore        | margaret.moore@sakilacustomer.org
 Dorothy Taylor        | dorothy.taylor@sakilacustomer.org
 Lisa Anderson         | lisa.anderson@sakilacustomer.org
 Nancy Thomas          | nancy.thomas@sakilacustomer.org
```

- We used the concatenation operator `||` to concatenate the first name, space, and last name of every customer.


### 5. Using PostgreSQl SELECT statement with expressions example

The following example uses the `SELECT` statement with an expression. It omits the `FROM` clause:


```sql
SELECT 5 * 3

 ?column? 
----------
       15
(1 row)

```
