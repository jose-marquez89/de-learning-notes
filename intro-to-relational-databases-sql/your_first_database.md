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
TODO