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

### How to implement N:M relationships
- create a table
- add a foreign key for every connected table
- add additional attributes

In the universities/professors/affiliations model, `n` professors can be associated with `m` organizations (chairman of finance, director of fundraising etc).

If you wanted to model this relationship you could create a table that looks like the following:
```sql
-- notice that there are no primary keys!
CREATE TABLE affiliations (
    professor_id integer REFERENCES professors (id),
    organization_id varchar(256) REFERENCES organization (id),
    affiliate_function varchar(256)
);
```

How to update columns of a table based on values from another table
```sql
-- this query only makes sense when there is only
-- one matching row in table_b
UPDATE table_a
SET column_to_update = table_b.column_to_update_from
FROM table_b
WHERE condition_1 AND condition_2 AND ...;
```

## Referential integrity
- A record referencing another table must refer to an existing record in that table
- specified between two table
- enforced through foreign keys

### Referential integrity violations
Referential integrity from table A to table B is violeted when:
- if a record in table B that is referenced from a record in table A is deleted
- if you insert a record into table A that references a non-existent record in table B
- **foreign keys prevent you from accidentally making these kinds of violations**

### Handling violations
```sql
-- ON DELETE is by default automatically added to a foreign key definition
CREATE TABLE a (
    id integer PRIMARY KEY,
    column_a varchar(64),
    ...,
    b_id integer REFERENCES b (id) ON DELETE NO ACTION
);
```
There are other alternatives:
```sql
-- the CASCADE options allows deletion of the record in table b 
-- but also goes and deletes referenced records in table a
CREATE TABLE a (
    id integer PRIMARY KEY,
    column_a varchar(64),
    ...,
    b_id integer REFERENCES b (id) ON DELETE CASCADE
)
```
Options for `ON DELETE`:
- `NO ACTION`: throws an error
- `CASCADE`: deletes all referencing records
- `RESTRICT`: throws an error
- `SET NULL`: set the referencing column to `NULL`
- `SET DEFAULT`: set the referencing column to a pre-defined default

### Additional notes
- altering a key constrain doesn't work with `ALTER COLUMN` 
    - you need to `DROP` the key constraint and then `ADD` a new one with a different `ON DELETE` behavior
    - you need to know the name of the constraint when you try to delete it
    - you can find these names in the `information_schema`