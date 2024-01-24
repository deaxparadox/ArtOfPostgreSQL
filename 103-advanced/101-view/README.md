# PostgreSQL Views

A view is a named query that provides another way to present data in the database tables. A view is defined based on one or more tables which are known as base tables.

When you create a view, you basically create a query and assign a name to the query. Therefore, a view is useful for wrapping commonly used complex query.


Note that regular views do not store any data except the materialized views. In PostgreSQL, you can create special views called materialized views that store data physically and periodically refresh data from the base tables. The materialized views are handy in many scenarios, such as faster data access to a remote server and caching.


- [Managing PostgreSQL views](101-managing-views.md): introduce you to the concept of the view and show you how to creating, modifying, and removing views from the database.
- [Drop View](102-drop-views.md): drop one or more views from the database.
- [Creating Updatable Views](103-creating-updatable-views.md): How to create updatable views in PostgreSQL