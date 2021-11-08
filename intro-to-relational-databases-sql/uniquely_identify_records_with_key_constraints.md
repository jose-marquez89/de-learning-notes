# Uniquely Identify Records With Key Constraints

## Keys and Superkeys
A key uniquely identifies a record
- normally a table contains only unique records
- superkey can be a combination of all attributes
    - if you remove an attribute (read "column" or "feature") and can still uniquely identify records, you still have a superkey
- a minimal superkey is one where you can't remove any more attributes or you'll lose uniqueness
- a minimal superkey can be a _candidate_ key, which is called such because you _choose_ a key from the candidates

There's a very basic way of finding out what qualifies for a key in an existing, populated table:

    Count the distinct records for all possible combinations of columns. If the resulting number x equals the number of all rows in the table for a combination, you have discovered a superkey.

    Then remove one column after another until you can no longer remove columns without seeing the number x decrease. If that is the case, you have discovered a (candidate) key.

## Primary Keys
Looking at use cases for superkeys, keys and candidate keys
- main purpose is to uniquely identify records in a table
- there's one primary key per database table
- Unique and non-null constraints both apply
- time invariant: constraints hold for current and future data

### Specifying primary keys
```sql
-- the two tables below accept the same exact data except that the latter
-- has an explicitly defined primary key
CREATE TABLE products (
    product_no UNIQUE NOT NULL,
    name text,
    price numeric
);

CREATE TABLE products (
    product_no PRIMARY KEY,
    name text,
    price numeric
);

-- if you want to designate more than one column as the primary key
CREATE TABLE example (
    a integer,
    b integer,
    c integer,
    PRIMARY KEY (a, c)
);
```
_Note: ideally, primary keys consist of as few keys as possible_

Adding a primary key constraint to an existing table
```sql
ALTER TABLE table_name
ADD CONSTRAINT some_name PRIMARY KEY (column_name) 
```

### Surrogate Keys
TODO