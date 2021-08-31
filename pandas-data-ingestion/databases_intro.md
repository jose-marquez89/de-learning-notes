# Intro to Databases

### Relational DB's
- entity data is organized into tables
- each row is (should be) an instance of an entity
- columns are information about attributes
- tables are linked via unique keys
- they support more data, simultaneous users, and data quality controls
- data types are specified for each column
- use structured query language for interaction

### Connecting to Databases
1. connect
2. query

#### Using SQLAlchemy
- use `create_engine()` to handle connections
- sqlite format for connection string: `sqlite:///filename.db`
- once the connection is made you can use `pd.read_sql(query, engine)` 

#### Basic SQL syntax
```sql
SELECT [column_names] FROM [table_name];

-- Get all rows and columns
SELECT * FROM [table_name];
```

#### Example
```python
import pandas as pd
from sqlalchemy import create_engine

engine = create_engine("sqlite:///data.db")

# you can either use the table name as the query OR
weather = pd.read_sql("weather", engine)

# you can use an actual query
q = "SELECT * FROM weather;"
weather = pd.read_sql(q, engine)
```
### Refining imports with SQL queries
- you can refine `read_sql()` calls with SQL

**Example**
```python
import pandas as pd
from sqlalchemy import create_engine

engine = create_engine("sqlite:///data.db")

query = """SELECT * 
           FROM hpd311calls
           WHERE borough = 'BROOKLYN';"""
brooklyn_calls = pd.read_sql(query, engine)
print(brooklyn_calls.borough.unique())
```

### More complex SQL queries

#### Unique values
- you can use `SELECT DISTINCT` in a pandas sql query to get unique values
- `SELECT DISTINCT [column names] FROM [table];`
- remove duplicates `SELECT DISTINCT * FROM [table];

Example
```sql
-- Get unique street addresses and boroughs
SELECT DISTINCT incident_address,
    borough
FROM hpd311calls;
```
#### Aggregate functions
A few examples:
- `SUM`
- `AVG`
- `MAX`
- `MIN`
- `COUNT`

First four above each take a single column name in parentheses:
```SQL
SELECT AVG(tmax) FROM weather;
```
`COUNT` is different:
```sql
-- get num of rows that meet query conditions 
SELECT COUNT(*) FROM [table_name];

-- get num of unique values in a column
SELECT COUNT(DISTINCT [column_names]) FROM [table_name];
```

#### Group by
- aggregate functions return a single summary stat
- `GROUP BY` lets you summarize by categories
- you need to select the column you're grouping by

Counting by groups using pandas and SQL:
```python
engine = create_engine("sqlite:///data.db")

query = """SELECT borough, COUNT(*)
           FROM hpd311calls
           WHERE complaint_type = 'PLUMBING'
           GROUP BY borough;"""

plumbing_call_counts = pd.read_sql(query, engine)
```

### Loading multiple tables with joins
- keys let you join separate tables
- key columns need to have the same data type

**Joining and Aggregating**
```sql
SELECT hpd311calls.borough,
    COUNT(*),
    boro_census.total_population,
    boro_census.housing_units,
FROM hpd311calls
JOIN boro_census
ON hpd311calls.borough = boro_census.borough
GROUP BY hpd311calls.borough;
```
_Something I'm not sure about SQL: when you group by a particular category, do numeric columns included in the group by get summed by default?_ 