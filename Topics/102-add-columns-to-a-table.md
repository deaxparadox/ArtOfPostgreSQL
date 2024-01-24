
- First,  specify the name of the table that you want to add a new column to after the `ALTER TABLE` keyword.
- Second, specify the name of the new column as well as its data type and constraint after the `ADD COLUMN` keywords.

When you add a new column to the table, PostgreSQL appends it at the end of the table. PostgreSQL has no option to specify the position of the new column in the table.

To add multiple columns to an existing table, you use multiple `ADD COLUMN` clauses in the `ALTER TABLE` statement as follows:


```sql
ALTER TABLE table_name
ADD COLUMN column_name1 data_type constraint,
ADD COLUMN column_name2 data_type constraint,
...
ADD COLUMN column_namen data_type constraint;
```


### PostgreSQL ADD COLUMN statement examples


The following `CREATE TABLE` statement creates a new table named customers with two columns: `id` and `customer_name`:

```sql
DROP TABLE IF EXISTS customers CASCADE;

CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    customer_name VARCHAR NOT NULL
);
```

