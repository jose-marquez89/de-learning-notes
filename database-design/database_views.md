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

