# Parallel Computing

## Hadoop
- maintained by Apache
- MapReduce and HDFS

### HDFS
- distributed file system
- often replaced by things like S3

### MapReduce
- was difficult to write jobs
- Hive popped up to address this

### Hive
- Runs on Hadoop
- built from the need to use structured queries for parallel processing

```SQL
SELECT year, AVG(age)
FROM views.athelete_events
GROUP BY year
```

### Spark
- Tries to keep as much processing in memory
- as opposed to expensive disk-writes
- maintained by Apache
- relies on resilient distributed datasets
  - like lists of tuples
  - operations on these structures
    - transformations `.map()` `.filter()`
      - result in transformed rdd's
    - actions `.count()` or `.first()`
      - result in single results
  - pyspark
    - like pandas dataframes
- probably more popular than dask for big data
- uses lazy evaluation
  
Example:
```python
(athlete_events_spark
  .groupBy('Year')
  .mean('Age')
  .show())
  
# some more examples
# Print the type of athlete_events_spark
print(type(athlete_events_spark))

# Print the schema of athlete_events_spark
print(athlete_events_spark.printSchema())

# Group by the Year, and find the mean Age
print(athlete_events_spark.groupBy('Year').mean('Age'))

# Group by the Year, and find the mean Age
print(athlete_events_spark.groupBy('Year').mean('Age').show())
```

**Submitting to a spark cluster**
```python
from pyspark.sql import SparkSession

if __name__ == "__main__":
    spark = SparkSession.builder.getOrCreate()
    athlete_events_spark = (spark
        .read
        .csv(<fpath>,
             header=True,
             inferSchema=True,
             escape='"'))
     
     athelete_events_spark = (athlete_events_spark
         .withColumn("Height",
                     athlete_events_spark.Height.cast("Integer"))
                     
     print(athlete_events_spark
         .groupBy('Year')
         .mean('Height')
         .orderBy('Year')
         .show())
```



