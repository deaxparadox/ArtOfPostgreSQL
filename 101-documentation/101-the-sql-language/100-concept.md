# SQL Language Comcept


PostgreSQL is a *relational database management system (RDBMS)*. That means it is a system for managing data stored in *relations*. Relation is essentially a mathematical term for *table*.

Each table if a named collection of *rows*. Each row of a given table has the same set of named *columns*, and each columns if of a specific data type. Whereas columns have a fixed order in each row, it is important to remember that SQL does not gurantee the order of the rows within the table in any way (although they can be explicitly sorted for display).

Tables are grouped into database, and a collection of databases managed by a single PostgreSQL server instance constitutes a database *cluster*.