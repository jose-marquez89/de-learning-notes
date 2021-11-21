**Merge This with "Glue together tables"**

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

