# Introduction to the PostgreSQL Views

A view is a stored query. A view can be accessed as virtual table in PostgreSQL. In other words, a PostgreSQL view is a logical table that represents data of one or more underlying tables through a SELECT statement.

Note that a view does not store data physically like a table except for a materialized view.


A view can be very useful in some cases such as:

- A view helps simplify the complexity of a query because you can query a view, which is based on a complex query, using a simple `SELECT` statement.
- Like a table, you can grant permission to users through a view that contains specific data that the users are authorized to see.
- A view provides a consistent layer even the columns of the underlying table change.

### Creating PostgreSQL Views

To create a view, we use `CREATE VIEW` statement. The simplest syntax of the `CREATE VIEW` statement is as follows:

```sql
CREATE VIEW view_name AS query;
```

First, you specify the name of the view after the `CREATE VIEW` clause, then you put a query after the `AS` keyword. A query can be a simple `SELECT` statement or a complex `SELECT` statement with joins.

#### PostgreSQL CREATE VIEW example

For example, in our sample database, we have four tables:

1. `customer` - stores all customer data
2. `address` - stores address of customers
3. `city` - stores city data
4. `country` - stores country data

If you want to join complete customers data, you normally construct a join immediately as follows:

```
 SELECT cu.customer_id AS id,
    cu.first_name || ' ' || cu.last_name AS name,
    a.address,
    a.postal_code AS "zip code",
    a.phone,
    city.city,
    country.country,
        CASE
            WHEN cu.activebool THEN 'active'
            ELSE ''
        END AS notes,
    cu.store_id AS sid
   FROM customer cu
     INNER JOIN address a USING (address_id)
     INNER JOIN city USING (city_id)
     INNER JOIN country USING (country_id);
```

The result of the query is as shown in the screeshot below:

```sql
 id  |         name          |                address                 | zip code |    phone     |            city            |         
       country                | notes  | sid 
-----+-----------------------+----------------------------------------+----------+--------------+----------------------------+---------
------------------------------+--------+-----
 524 | Jared Ely             | 1003 Qinhuangdao Street                | 25972    | 35533115997  | Purwakarta                 | Indonesia                             | active |   1
   1 | Mary Smith            | 1913 Hanoi Way                         | 35200    | 28303384290  | Sasebo                     | Japan                                 | active |   1
   2 | Patricia Johnson      | 1121 Loja Avenue                       | 17886    | 838635286649 | San Bernardino             | United States                         | active |   1
   3 | Linda Williams        | 692 Joliet Street                      | 83579    | 448477190408 | Athenai                    | Greece                                | active |   1
   4 | Barbara Jones         | 1566 Inegl Manor                       | 53561    | 705814003527 | Myingyan                   | Myanmar                               | active |   2
   5 | Elizabeth Brown       | 53 Idfu Parkway                        | 42399    | 10655648674  | Nantou                     | Taiwan                                | active |   1
   6 | Jennifer Davis        | 1795 Santiago de Compostela Way        | 18743    | 860452626434 | Laredo                     | United States                         | active |   2
   7 | Maria Miller          | 900 Santiago de Compostela Parkway     | 93896    | 716571220373 | Kragujevac                 | Yugoslavia                            | active |   1
   8 | Susan Wilson          | 478 Joliet Way                         | 77948    | 657282285970 | Hamilton                   | New Zealand                           | active |   2
   9 | Margaret Moore        | 613 Korolev Drive                      | 45844    | 380657522649 | Masqat                     | Oman                                  | active |   2
  10 | Dorothy Taylor        | 1531 Sal Drive                         | 53628    | 648856936185 | Esfahan                    | Iran                                  | active |   1
```

This query is quite complex. However, you can create a view named `customer_master` as follows:

```sql
CREATE VIEW customer_master AS
  SELECT cu.customer_id AS id,
    cu.first_name || ' ' || cu.last_name AS name,
    a.address,
    a.postal_code AS "zip code",
    a.phone,
    city.city,
    country.country,
        CASE
            WHEN cu.activebool THEN 'active'
            ELSE ''
        END AS notes,
    cu.store_id AS sid
   FROM customer cu
     INNER JOIN address a USING (address_id)
     INNER JOIN city USING (city_id)
     INNER JOIN country USING (country_id);
```

From onw on, whenever you need to get complete customer data, you just query it from the view by executing the following simple `SELECT` statement:

```sql
dvdrental=# select * from customer_master;
```

This query produces the same result as the complex on with the joins above.

### Changing PostgreSQL Views

To change the defining query of a view, you use the `CREATE VIEW` statement with  `OR REPLACE` addition as follows:

```sql
CREATE OR REPLACE view_name AS query;
```

PostgreSQL does not support removing an existing column in the view, at least up to version 9.4. If you try to do it, you will get an error message: “[Err] ERROR:  cannot drop columns from view”. The query must generate the same columns that were generated when the view was created. To be more specific, the new columns must have the same names, the same data types, and in the same order as they were created. However, PostgreSQL allows you to append additional columns at the end of the column list.

For example, you can add an email to the `cutomer_master` view as follows:

```sql
CREATE VIEW customer_master AS
  SELECT cu.customer_id AS id,
    cu.first_name || ' ' || cu.last_name AS name,
    a.address,
    a.postal_code AS "zip code",
    a.phone,
    city.city,
    country.country,
        CASE
            WHEN cu.activebool THEN 'active'
            ELSE ''
        END AS notes,
    cu.store_id AS sid,
    cu.email
   FROM customer cu
     INNER JOIN address a USING (address_id)
     INNER JOIN city USING (city_id)
     INNER JOIN country USING (country_id);
```

Now, if you select data from the `customer_master` view, you will see the `email` column at the end of the list.

```sql
SELECT
	*
FROM
	customer_master;
```

```output
 id  |         name          |                address                 | zip code |    phone     |            city            |                country                | notes  | sid 
-----+-----------------------+----------------------------------------+----------+--------------+----------------------------+---------------------------------------+--------+-----
 524 | Jared Ely             | 1003 Qinhuangdao Street                | 25972    | 35533115997  | Purwakarta                 | Indonesia                             | active |   1
   1 | Mary Smith            | 1913 Hanoi Way                         | 35200    | 28303384290  | Sasebo                     | Japan                                 | active |   1
   2 | Patricia Johnson      | 1121 Loja Avenue                       | 17886    | 838635286649 | San Bernardino             | United States                         | active |   1
   3 | Linda Williams        | 692 Joliet Street                      | 83579    | 448477190408 | Athenai                    | Greece                                | active |   1
   4 | Barbara Jones         | 1566 Inegl Manor                       | 53561    | 705814003527 | Myingyan                   | Myanmar                               | active |   2
   5 | Elizabeth Brown       | 53 Idfu Parkway                        | 42399    | 10655648674  | Nantou                     | Taiwan                                | active |   1
   6 | Jennifer Davis        | 1795 Santiago de Compostela Way        | 18743    | 860452626434 | Laredo                     | United States                         | active |   2
   7 | Maria Miller          | 900 Santiago de Compostela Parkway     | 93896    | 716571220373 | Kragujevac                 | Yugoslavia                            | active |   1
   8 | Susan Wilson          | 478 Joliet Way                         | 77948    | 657282285970 | Hamilton                   | New Zealand                           | active |   2
   9 | Margaret Moore        | 613 Korolev Drive                      | 45844    | 380657522649 | Masqat                     | Oman                                  | active |   2
  10 | Dorothy Taylor        | 1531 Sal Drive                         | 53628    | 648856936185 | Esfahan                    | Iran 
```

To change the definition of a view, you use the `ALTER VIEW` statement. For example, you can change the name of the view from `customer_master` to `customer_info` by using the following statement:

```sql
ALTER VIEW customer_master RENAME TO customer_info;
```

PostgreSQL allows you to set a default value for a column name, change the view's schema, set or reset options of a view.



### Removing PostgreSQL Views

To remove an existing view in PostgreSQL, you use `DROP VIEW` statement as follows:

```sql
DROP VIEW [ IF EXISTS ] view_name;
```

You specify the name of the view that you want to remove after `DROP VIEW` clause. Removing a view that does not exist in the database will result in an error. To avoid this, you normally add `IF EXISTS` option to the statement to instruct PostgreSQL to remove the view if it exists, otherwise, do nothing;

For example, to remove the `customer_info` view that you have created, you execute the following query:

```sql
DROP VIEW IF EXISTS customer_info;
```

The view `customer_info` is removed from the database.