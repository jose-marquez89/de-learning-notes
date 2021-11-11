# Glue together tables with foreign keys

## Foreign keys
These are designated columns that point to the primary key of another column.

### Restrictions
- domain of foreign key datatype must be the same as for the primary key that it will point to
- only foreign keys are allowed that exist as values in the primary key of the reference table
    - "referential integrity"
- FK's are not actual _keys_
    - this is because duplicates and null values are allowed

### Specifying foregin keys
```sql
CREATE TABLE manufacturers (
    name varchar(255) PRIMARY KEY);

INSERT INTO manufacterers
VALUES ('Ford'), ('VW'), ('GM'); 

CREATE TABLE cars (
    model varchar(255) PRIMARY KEY,
    manufacturer_name varchar(255) REFERENCES manufacturers (name));

INSERT INTO cars 
VALUES ('Ranger', 'Ford'), ('Beetle', 'VW');
```
The following would throw an error if you tried to execute based on the SQL above
```sql
INSERT INTO cars
VALUES ('Tundra', 'Toyota')
```

### Specifying foreign keys to existing tables
```sql
ALTER TABLE a
ADD CONSTRAINT a_fkey FOREIGN KEY (b_id) REFERENCES b (id);
```

## Model more complex relationships