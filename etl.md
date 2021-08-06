# ETL process

### Extract
- extracting from persistent storage that isn't suited for data processing
- plaintext unstructured or flatfiles
- json semistructured
  - atomic
    - number
    - string
    - boolean
    - null
  - composite
    - array
    - object

### Web
- requests can come through as JSON from API's
- python requests

### Databases
- most web services use one to persist data
- Appication dbs
  - transactions insert or change data
  - OLTP: online transaction processing
  - typically row oriented
- analytical dbs
  - OLAP: online analytical processing
  - often column oriented
- you need a connection string for python
- you can google what these look like
- sqlalchemy is common
