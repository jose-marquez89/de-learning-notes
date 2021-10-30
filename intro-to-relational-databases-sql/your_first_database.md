# Your first database

## Intro to Relational databases
- real life _entities_ become _tables_
- reduced redundancy

Three important concepts
- constraints
- keys
- referential integrity

### Information schema
The information schema is a sort of meta database that has info about your current database
```sql
SELECT table_schema, table_name
FROM information_schema.tables;
```
You can get column data by looking at the information schema for a table's columns
```sql
SELECT table_name, column_name, data_type
FROM information_schema.columns
WHERE table_name = 'pg_config';
```

For your purposes, you will usually only be concerned with 'public' schema which holds user-defined tables and databases

## Tables: at the core of every database

### Creating a table
```sql
CREATE TABLE weather (
    clouds text,
    temperature numeric,
    weather_station char(5)
)
```

If a table is still empty and you need to add a column you can do so (you can still do it if the table isn't empty but things get a little more complex):
```sql
ALTER TABLE table_name
ADD COLUMN column_name data_type;
```

### Updating the database as the structure changes
Inserting new distinct records
```sql
INSERT INTO organizations
SELECT DISTINCT organization,
    organization_sector
FROM university_professors;
```

Inserting values manually
```sql
INSERT INTO table_name (column_a, column_b)
VALUES ("value_a", "value_b")
```

Rename a column
```sql
CREATE TABLE affiliations (
    first_name text,
    last_name text,
    university_shortname text,
    function text,
    organisation text
);

-- Rename the 'organisation' column
ALTER TABLE table_name
RENAME COLUMN old_name TO new_name;
```

Drop a column
```sql
ALTER TABLE table_name
DROP COLUMN column_name
```
You should take care to reduce redundancy in databases. If there's a column that's redundant in terms of referencing some entity, remove it.

### Better data quality with constraints
TODO