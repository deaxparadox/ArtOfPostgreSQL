# Accessing a Database

Once you have created a database, you can access it by:

- Running the PostgreSQL interactive terminal program, call *psql*, which allows you to interactively enter, edit, and execute SQL commands.
- Using an existing graphical frontend tool like pgAdmin or an office suite with ODBC or JDBC support to create and manipulate a database. 
- Writing a custom application, using one of the several available language bindings.


To access *mydb* database:

```sql
$ psql mydb
```

```output
psql (15.2 (Debian 15.2-1.pgdg110+1))
Type "help" for help.

mydb=# 
```

Try out these commands:

```sql
mydb=# SELECT version();
```

```output
-[ RECORD 1 ]------------------------------------------------------------------------------------------------------------------------
version | PostgreSQL 15.2 (Debian 15.2-1.pgdg110+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 10.2.1-6) 10.2.1 20210110, 64-bit
```

```sql
mydb=# SELECT current_date;
```

```output
-[ RECORD 1 ]+-----------
current_date | 2023-05-01
```

```sql
mydb=# SELECT 2+2;
```

```output
-[ RECORD 1 ]
?column? | 4
```

you can get help on the syntax of various PostgreSQL SQL commands by typing;

```sql
mydb=> \h
```

To get out of psql, type:

```sql
mydb=> \q
```
