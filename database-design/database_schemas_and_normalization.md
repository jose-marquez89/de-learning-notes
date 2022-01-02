# Database Schemas and Normalization

## Star and Snowflake schema

### Star Schema
Star schema and "dimensional model" can often mean the same thing
- star schemas extend one dimension out from the fact table
- they tend to look like stars (go figure)

### Snowflake Schema
- has a different structure for the dimension tables
- extends over more than one dimension
- each dimension table can have it's own dimension table recursively (up to a point)
- can have the same data as a star schema model but normalization will allow for the extension of what used to be just one dimension

### Normalization
- a database design technique
- divides tables into smaller tables and connects them via relationships
- the **goal** is to reduce data redundancy and increase data integrity
- starting point is to identify repeating groups of data and create new tables for them
- you are essentially breaking things down to where the root dimension table is the finest level of granularity in terms of hierarchy

Refresher on adding constraints
```sql
-- Add the book_id foreign key
ALTER TABLE fact_booksales ADD CONSTRAINT sales_book
    FOREIGN KEY (book_id) REFERENCES dim_book_star (book_id);
    
-- Add the time_id foreign key
ALTER TABLE fact_booksales ADD CONSTRAINT sales_time
    FOREIGN KEY (time_id) REFERENCES dim_time_star (time_id);
    
-- Add the store_id foreign key
ALTER TABLE fact_booksales ADD CONSTRAINT sales_store
    FOREIGN KEY (store_id) REFERENCES dim_store_star (store_id);
```

Creating a dimension table for book authors from a book sales table
```sql
-- Create a new table for dim_author with an author column
CREATE TABLE dim_author (
    author varchar(256)  NOT NULL
);

-- Insert authors 
INSERT INTO dim_author
SELECT DISTINCT author FROM dim_book_star;

-- Add a primary key 
ALTER TABLE dim_author ADD COLUMN author_id SERIAL PRIMARY KEY;

-- Output the new table
SELECT * FROM dim_author;
```

## Normalized and Denormalized Databases
Normalized schemas will usually require more complex queries for analysis, so why would we normalize a database?
- normalization saves space
- denormalization enables data redundancy
    - this saves less space but queries are less complex and often more performant

### Normalization ensures better data integrity
1. enforces data consistency
    - referential integrity will keep spelling mistakes and things of that nature from making it into a table
2. safer updating, removing and inserting
    - less redundancy = less records to alter
3. easier to redesign by extending
    - small tables are easier to extend than larger tables

### Pros and Cons of Normalization
Pros
- elimination of redundancy, saves space
- better data integrity, accurate and consistent data

Cons
- complex queries require more CPU

### Normalization and OLTP/OLAP
OLTP (e.g. operational databases)
- typically very normalized
- lots of writing goes on
- quick and safe insertion of data is priority

OLAP (e.g. data warehouses)
- read-intensive
- priority is on quicker queries

### SQL refresher
Adding a new column to a snowflake schema table that references a new hierarchy via a foreign key constraint
```sql
-- Add a continent_id column with default value of 1
ALTER TABLE dim_country_sf
ADD COLUMN continent_id int NOT NULL DEFAULT(1);

-- Add the foreign key constraint
ALTER TABLE dim_country_sf ADD CONSTRAINT country_continent
   FOREIGN KEY (continent_id) REFERENCES dim_continent_sf(continent_id);
   
-- Output updated table
SELECT * FROM dim_country_sf;
```

## Normal Forms
Normalization is basically the identification of repeating groups of data and the creation of new tables for them

More formal definition:
- be able to characterize the level of redundancy in a relational schema
- provide mechanisms for transforming schemas in order to reduce redundancy

_above is from 'Database Design, 2nd Edition', Adrienne Watt_

Normal forms are the levels of extent to which you can normalize data, here's a list from least to most normalized:
- first normal form (1NF)
- second normal form (2NF)
- third normal form (2NF)
- elementary key normal form (EKNF)
- Boyce-Codd normal form (BCNF)
- fourth normal form (4NF)
- essential tuple normal form (ETNF)
- fifth normal form (5NF)
- domain key normal form (DKNF)
- sixth normal form (6NF)

### 1NF
- each record is unique
- each cell holds one value

### 2NF
- must satisfy 1NF **AND**
    - if primary key is one column, 2NF is automatically satisfied
- if there is a composite primary key
    - each non-key column must be dependent on all keys
    - in other words, a column that is dependent on only one piece of the composite key makes the table not satisfy 2NF

### 3NF
- satisfies 2NF
- does not allow for **transitive dependencies**
    - non-key columns can't depend on other non-key columns

### Data anomalies
If you don't normalize a database enough, it is prone to 3 types of anomaly errors:
1. update anomaly
    - this happens when there is a need to update more than one record
    - the user would have to be aware of the data redundancy to make sure all records are updated
2. insertion anomaly
    - this happens when you're unable to add a new record due to missing attributes
    - if a column doesn't accept nulls, you won't be able to add the new record if it has no data for a particular column
3. deletion anomaly
    - happens when you delete a record and unintentionally delete other data

_Remember that there are downsides to normalization!_
