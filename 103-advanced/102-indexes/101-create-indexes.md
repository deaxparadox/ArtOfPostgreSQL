# PostgreSQL CREATE INDEX

### Phonebook analogy and indexs

Suppose you need to look up `John Doe`'s phone number in the phone book. Assuming that the names on the phone book are in alphabetical order. To find `John Doe`'s phone number, you first look for the page where the last name is `Doe`, then look for the first name `John`, and finally, get his phone number.

If the names on the phone book were not ordered alphabetically, you would have to go through all pages and check every name untill you find John Doe's phone number. This is called sequential scan which you go over all entries until you find the one that you are looking for.

By definition, an index is a separated data structure that speeds up the data retrieval on a table at the cost of additional writes and storage to maintain the index.

### PostgreSQL CREATE INDEX overview

The following show the basic syntax of the `CREATE INDEX` statement:

```sql
CREATE INDEX index_name ON table_name [USING method]
(
    column_name [ASC | DESC] [NULLS {FIRST | LAST }],
    ...
);
```