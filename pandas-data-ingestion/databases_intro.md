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