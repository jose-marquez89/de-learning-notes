# Database Management 

## Database Roles and Access Control
- roles allow you to manage access permissions
- a role is an entity that contains information that:
    - defines the role's priveleges ->
        - can you login?
        - can you create databases?
        - can you write to tables?
    - interacts with the client auth system
        - password
- roles can be assigned to one or more users
- roles are global across a database cluster installation

Creating an empty role
```sql
CREATE ROLE data_analyst;
```

Creating some roles with attributes set
```sql
CREATE ROLE intern WITH PASSWORD 'PasswordForInter' VALID UNTIL '2021-01-01';

CREATE ROLE admin CREATEDB;

-- allow new admin to also create roles
ALTER ROLE admin CREATEROLE;
```

### GRANT and REVOKE priveleges from roles 
You can grant priveleges on objects like views and schemas with `GRANT` and `REVOKE`

Example: You want all your data analysts to have permission to update the `ratings` view:
```sql
GRANT UPDATE ON ratings TO data_analyst;

-- when you don't need it anymore
REVOKE UPDATE ON ratings FROM data_analyst;
```

Available priveleges in PostgreSQL:
- `SELECT`
- `INSERT`
- `UPDATE`
- `DELETE`
- `TRUNCATE`
- `REFERENCES`
- `TRIGGER`
- `CREATE`
- `CONNECT`
- `TEMPORARY`
- `EXECUTE`
- `USAGE`

### Users and groups are both roles
- a role is an entity that can function as a user and/or group
    - user roles
    - group roles

Group role
```sql
CREATE ROLE data_analyst;
```

User role
```sql
-- create a role for the user alex
CREATE ROLE alex WITH PASSWORD 'PasswordForIntern' VALID UNTIL '2021-01-01';

-- grant the analyst role to user alex
GRANT data_analyst TO alex;

-- when no longer needed
REVOKE data_analyst TO alex;
```

Postgres has a set of default roles
|**Role**|**Allowed access**|
|--------|------------------|
|pg_read_all_settings|Read all configuration variables, even those normally visible only to superusers|
|pg_read_all_stats|Read all pg_stat_* views and use various statistics related extensions, even those normally only visible to superusers|
|pg_signal_backend|Send signals to other backends (eg:cancel query, terminate)|
|More...|More...|

### Benefits and Pitfalls of roles
**Benefits**
- Roles live on after users are deleted
- Roles can be created before user accounts
- save DBA time

**Pitfalls**
- sometimes a role will give an individual too much access
    - you need to be very mindful of this

## Table Partitioning
Why you might want to partition tables
- some tables will get very large in size
- indices will start to not fit in memory
- when this happens you can _partition_ tables
    - partitioning the table means splitting it up into smaller chunks
- the _logical_ (see notes on procesing data) data model stays the same when you partition
- partitioning is a part of the physical data model

### Vertical partitioning
This is when you partition by columns
- a table may be split up in such a way that you end up with two tables
- the second table may be a column or group of columns that are not queried very often
- these columns can be linked with an id from the original unpartitioned table
- you can store the new table on a slower medium

### Horizontal partitioning
This is when you partiiton by rows
- you can split up a table's rows according to it's time stamp

#### Declarative Partitioning (PostgreSQL 10)
```sql
CREATE TABLE sales (
    ...
    timestamp DATE NOT NULL
)
PARTITION BY RANGE (timestamp);

CREATE TABLE sales_2019_q1 PARTITION OF sales
    FOR VALUES FROM ('2019-01-01') TO ('2019-03-31');
...
CREATE TABLE sales_2019_q4 PARTITION OF sales
    FOR VALUES FROM ('2019-09-01') TO ('2019-12-31');
CREATE INDEX ON sales ('timestamp');
```

### Pros/Cons of Partitioning
Pros
- indices of heavily used partitions can fit into memory
- you can move to a specific medium: slower vs faster
- beneficial for both OLAP and OLTP

Cons
- partitioning an existing table can be a hassle
    - you have to copy over the data to a new table
- you won't be able to set certain constriaints (like PRIMARY KEY)

### Relation to sharding
This takes horizontal partitioning one step further
- tables are spread over several machines
- this is called sharding
- related to massively parallel processing db's
    - each node (machine) does calculations on specific shards

Vertical partitioning example from exercises:
```sql
-- Create a new table called film_descriptions
CREATE TABLE film_descriptions (
    film_id INT,
    long_description TEXT
);

-- Copy the descriptions from the film table
INSERT INTO film_descriptions
SELECT film_id, long_description FROM film;
    
-- Drop the descriptions from the original table
ALTER TABLE film DROP COLUMN long_description;

-- Join to view the original table
SELECT * FROM film 
JOIN film_descriptions USING(film_id);
```

Example of horizontal partitioning
```sql
-- Create a new table called film_partitioned
CREATE TABLE film_partitioned (
  film_id INT,
  title TEXT NOT NULL,
  release_year TEXT
)
PARTITION BY LIST (release_year);

-- Create the partitions for 2019, 2018, and 2017
CREATE TABLE film_2019
	PARTITION OF film_partitioned FOR VALUES IN ('2019');

CREATE TABLE film_2018
	PARTITION OF film_partitioned FOR VALUES IN ('2018');

CREATE TABLE film_2017
	PARTITION OF film_partitioned FOR VALUES IN ('2017');

-- Insert the data into film_partitioned
INSERT INTO film_partitioned
SELECT film_id, title, release_year FROM film;

-- View film_partitioned
SELECT * FROM film_partitioned;
```

## Data integration
What is data integration? It's the combination of data from different sources, formats and technologies to provide users with a translated and unified view of that data.

### Business use cases
- a 360 degree customer view
    - a company wants to see data for all their customers from all departments in a unified view
- Acquisition
    - a company needs to integrate data from the database of a company they've acquired
- Legacy systems
    - a company needs to see data from a legacy system along with new systems all at once in a query

### Things to consider for integration
- cadence
    - how often does the data need to refresh?
- transformation
    - different data sources will have different formats
    - you can transform each source individually and maintain that code or you can use a tool that helps with data integration like **Apache Airflow** or **Scriptella**
- choosing tools
    - flexible: support many data sources
    - reliable: still going to be able to maintain in a year
    - scalable: anticipate increase in data volume and sources
- automated testing and alerts
    - you can do things like make sure that totals are still the same after aggregation
- security
    - if analysts didn't have access to a certain portion of the data in the original source, they shouldn't have access in the unified data model
- data governance
    - consider the lineage
    - know where the data came from and where it is used

## Picking a Database Management 

### DBMS
A DBMS is system software for creating and maintaining databases. It serves as an interface between the database and end users or applications.

Depending on your needs you may end up using a SQL DBMS or a NoSQL DBMS

### SQL DBMS
- based on a relational model
- SQL Server, PostgreSQL, Oracle SQL
- good option when working with structured, unchanging data that benefits from an unchanging schema
- good for things like accounting systems

### NoSQL DBMS
- much less structured than SQL DBMS
- document-centered
- good for situations where there is rapid data growth with no predefined schema
- generally classified as either of four types
    - key-value store
    - document-store
    - columnar
    - graph database

#### Key-Value store
- example: Redis
- used for managing session information in web apps
- key value pairs like a python dictionary or JSON

#### Document-store
- similar to key-value store except that the stored values are documents that provide some structure and encoding
- the strucutre can be used for more advanced queries than just simple value retrieval
- good choice for content management platforms such as blogs and video platforms
- an entity that an app is tracking can be a single document
- mongoDB is an example of a document store DBMS

#### Graph database
- data is interconnected and best represented by a graph
- used by applications like social networks and websites that recommend things based on your behavior
- example Neo4j

### Choosing a DBMS
- fixed business structure: SQL
- data is changing frequently and growing rapidly: NoSQL