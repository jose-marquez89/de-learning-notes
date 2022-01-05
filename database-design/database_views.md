# Database Views
A view is basically a stored query that is not part of the physical schema
- the query, not the data is stored
- can be queried like a regular table
- no need to retype common queries or alter schemas

### Creating views
```sql
CREATE VIEW view_name AS
SELECT col1, col2
FROM table_name
WHERE condition;
```

### Viewing views
This is specific to PostgreSQL, look at docs for other DBMS
```sql
-- includes system views
SELECT * FROM INFORMATION_SCHEMA.views;

-- exclude system views
SELECT * FROM INFORMATION_SCHEMA.views;
WHERE table_schema NOT IN ('pg_catalog', 'information_schema');
```

#### Benefits of views
- doesn't take up storage
- it can be a form of **access control**
    - hide sensitive columns and restrict what users can see
- masks complexity of queries
    - useful for schemas that have been normalized very deeply

## Managing views

### Granting and revoking access to views
You can use `GRANT privelege(s)` or `REVOKE privelege(s)`
`ON object`
`TO role` or `FROM role`
- priveleges are `SELECT`, `INSERT`, `UPDATE`, `DELETE`
- objects are tables, views, schemas
- roles are database users or groups of users

Granting update priveleges on 'ratings' for all users:
```sql
GRANT UPDATE ON ratings TO PUBLIC;
```

Similarly:
```sql
REVOKE INSERT ON films FROM db_user;
``` 

### Updating a view 
A user can update a view if they have the necessary priveleges. Here's an example: 
```sql 
-- remember that not all views can be updated 
-- you're really updating the table behind the view
UPDATE films SET kind = 'Dramatic' WHERE kind = 'Drama';
```
When can you update a view?
- view consists of only one table
- doesn't use a window or aggregate function

### Inserting into a view
- not all views are insertable
- generally, it's a good idea to use views for read-only purposes
- avoid data modification through views

### Dropping a view
```sql
DROP VIEW view_name [ CASCADE | RESTRICT ];
```
Sometimes views are built on other views, this is not uncommon
- `RESTRICT`: returns an error if there are objects that depend on the view
- `CASCADE`: drops view and all dependent views

### Redefining a view
```sql
CREATE OR REPLACE VIEW view_name AS new_query;
```
- if a view with the specified name exists, it will be replaced
- `new_query` must generate the same column names, order and data types as the old query
- the column output may be different
- new columns may be added at the end

If the new view cannot meet the above specifications, drop the view and create a new one

### Altering a view
You can alter auxiliary properties of a view:
```sql
ALTER VIEW [ IF EXISTS ] name ALTER [ COLUMN ] column_name SET DEFAULT expression       
ALTER VIEW [ IF EXISTS ] name ALTER [ COLUMN ] column_name DROP DEFAULT
ALTER VIEW [ IF EXISTS ] name OWNER TO new_owner
ALTER VIEW [ IF EXISTS ] name RENAME TO new_name
ALTER VIEW [ IF EXISTS ] name SET SCHEMA new_schema
ALTER VIEW [ IF EXISTS ] name SET ( view_option_name [= view_option_value] [, ... ])
ALTER VIEW [ IF EXISTS ] name RESET (view_option_name ) [, ... ])
```

You can check to see which views can be updated:
```sql
SELECT * FROM information_schema.views
WHERE is_updatable = 'YES' AND table_schema = 'public';
```

## Materialized views
There are two types of views
- views: aka non-materialized views
- materialized view: stored on disk, not virtual
    - can be refreshed on a schedule
    - refresh frequency depends on how often you expect the data to change
    - great if you have views with long execution time
    - caveat: the data is only as current as the last time it was refreshed
    - don't use on data that gets updated very often
        - this can lead to analyses that are run on out of date data on a frequent basis

### When to use materialized views
- long runing queries
- underlying query results don't change often
- data warehouses because OLAP is mostly read
    - saves on computational cost of running view queries

### Creating materialized views 
In PostgreSQL:
```sql
CREATE MATERIALIZED VIEW my_mv AS SELECT * FROM existing_table;
```
Refreshing:
```sql
REFRESH MATERIALIZED VIEW my_mv;
```
You can refresh materialized views on a schedule using something like cron jobs

### Managing dependencies
- materialized views often rely on other materialized views
- you need to manage when you refresh materialized views based on dependencies
- a query that depends on another with a long execution time may refresh with out-of-date data
- you will often end up with a dependency chain

### Tools for managing dependencies 
- many companies use DAG (Directed Acyclic Graph) to keep track of views
    - there are no cycles here, circular relationships are not part of DAGs
- pipeline scheduler tools like airflow and luigi are also used


