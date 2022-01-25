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