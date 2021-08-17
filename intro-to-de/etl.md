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

### Transform

Here's an example of splitting in pandas
```python
customer_df

split_email = customer_df.email.str.split("@", expand=True)

customer_df = customer_df.assign(
  username=split_email[0],
  domain=split_email[1]
)
```

#### Extracting data into PySpark
```python
import pyspart.sql

spark = pyspark.sql.SparkSession.builder.getOrCreate()

spark.read.jdbc("jdbc:postgresql://localhost:5432/pagila",
                "customer",
                properites={"user":"repl", "password":"password"})
```

#### A PySpark Join
```python
customer_df
ratings_df

ratings_per_customer = ratings_df.groupBy("customer_id").mean("rating")

customer_df.join(
  ratings_per_customer,
  customer_df.customer_id==ratings_per_customer.customer_id
)
```

_Note: Is .show() a pandas dataframe method as well as pyspark?_

### Load

#### Analytics or Applications?
- analytics
  - gets optimized for online analytical processing (OLAP)
  - lots of aggregate queries 
  - column oriented
    - better for parallelization
    - queries about subsets of columns
- applications
  - gets optimized for lots of transactions
  - online transaction processing (OLTP)
  - row oriented
    - stored per record
    - added per transaction
    - e.g. makes adding a customer fast

#### MPP (Massively Parallel Processing) Databases
- queries are split into subtasks and distributed among several nodes
- examples
  - amazon redshift
  - azure sql data warehouse
  - google bigquery

**Example: Redshift**
Load from file to columnar storage format
```python
# Pandas
df.to_parquet("./s3://path/to/bucket/customer.parquet")

# PySpark
df.write.parquet("./s3://path/to/bucket/customer.parquet")
```
Connect to Redshift
```
COPY customer
FROM './s3://path/to/bucket/customer.parquet'
FORMAT as parquet
```
Loading to PostgreSQL with `pandas.to_sql()`
```python
# transformation on data
recommendations = transform_find_recommendations(ratings_df)

# load into PostgreSQL database
recommendations.to_sql("recommendations",
                       db_engine,
                       schema="store",
                       if_exists="replace")
```

A typical workflow:
- write data to columnar files
- upload to storage system
- copy into a data warehouse from there
- in the case of Amazon Redshift, the storage system is S3

### Putting it all together
A typical macro level workflow
- create functions that extract, transform and load
- a main etl function runs all the other functions
- you can then manage a DAG using apache Airflow

#### Defining a DAG: Airflow
```python
from airflow.models import DAG
from airflow.operators.python_operator import PythonOperator

dag  = DAG(dag_id="etl_pipeline",
           schedule_interval="0 0 * * *")

etl_task = PythonOperator(task_id="etl_task",
                          python_callable=etl,
                          dag=dag)

etl_task.set_upstream(wait_for_this_task)
```
The file above would be saved as `etl_dag.py` in `~/airflow/dags`
You can go to [crontab.guru](https://crontab.guru) to learn more about cron expressions.
You can find instructions on running Airflow locally [here](https://airflow.apache.org/docs/apache-airflow/stable/start/index.html)
