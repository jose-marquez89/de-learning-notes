# Getting to know PySpark

## What is pyspark anyway
- basically a way to split up working with data across a cluster of nodes
- operations are performed in parallel

### Questions to ask before choosing spark
- is my data too big to work on a single machine
- can my calculations be easily parallelized

## Using spark in python
- first step is to connect a cluster
    - the _master_ computer sends data and calculations to be done by the _worker_ computers
- simpler to start with a local cluster for learning
- you create a connection by creating an instance of the `SparkContext` class
    - you can pass attributes to the class but there is also a `SparkConf()` constructor that can hold all these attributes

Some things you can do with a spark context
```python
# Verify SparkContext
print(sc)

# Print Spark version
print(sc.version)
```
## Using dataframes
- spark's core data structure is the RDD (resilient distributed dataset)
- DataFrames are abstractions built on the RDD's
    - easier to work with 
    - most optimizations are built in
    - can more easily handle complicated operations
    - behave similarly to SQL tables
- to work with dataframes you need to create a `SparkSession` object from the `SparkContext`  
- the `SparkSession` is like an interface to the `SparkContext` that is connecting you to the cluster

Starting a spark session
```python
# Import SparkSession from pyspark.sql
from pyspark.sql import SparkSession

# Create my_spark
my_spark = SparkSession.builder.getOrCreate()

# Print my_spark
print(my_spark)
```
You can list tables in spark with `spark.catalog.list_tables()`

#### SQL queries and Pandas dataframes
You can use sql queries on spark clusters
```python
query = "FROM flights SELECT * LIMIT 10"

# Get the first 10 rows of flights
flights10 = spark.sql(query)

# Show the results
flights10.show()
```
From SQL query to pandas DataFrame
```python
# Don't change this query
query = "SELECT origin, dest, COUNT(*) as N FROM flights GROUP BY origin, dest"

# Run the query
flight_counts = spark.sql(query)

# Convert the results to a pandas DataFrame
pd_counts = flight_counts.toPandas()

# Print the head of pd_counts
print(pd_counts.head())
```
You can also take a Pandas DataFrame and put it in your local spark session:
```python
# Create pd_temp
pd_temp = pd.DataFrame(np.random.random(10))

# Create spark_temp from pd_temp
spark_temp = spark.createDataFrame(pd_temp)

# Examine the tables in the catalog
print(spark.catalog.listTables())

# Add spark_temp to the catalog
spark_temp.createOrReplaceTempView("temp")

# Examine the tables in the catalog again
print(spark.catalog.listTables())
```
#### Read straight from a csv
You don't really need pandas to bring a file into the spark cluster (is it actually in the spark cluster, I'm not sure of this)
```python
# Don't change this file path
file_path = "/usr/local/share/datasets/airports.csv"

# Read in the airports data
airports = spark.read.csv(file_path, header=True)

# Show the data
airports.show()
```