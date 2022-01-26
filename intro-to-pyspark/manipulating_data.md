# Manipulating Data with Spark

## Performing column-wise operations
- you can use the `withColumn()` method
- new column needs to be a `Column` class
- you can call on a column with `<yourDataframe>.colname`
- Spark dataframes are immutable
- you can overwrite an existing column by passing the name of the column as the first argument to `.withColumn`

Overwriting the dataframe: 
```python
# you need to reassign the dataframe when you add a new column
df = df.withColumn("newCol", df.oldCol + 1)
```

## SQL in a nutshell

### Filter
- spark's counterpart of the SQL where clause is the `.filter` method
- `.filter()` can accept any expression that could go in the `WHERE` clause of a SQL query
    - if you use it this way, you must pass it in as a string
- you can also pass in a column of boolean values (see below)

Example:
```python
# both produce the same output
flights.filter("air_time > 120").show()
flights.filter(flights.air_time > 120).show()
```

### Select
```python
# Select the first set of columns
selected1 = flights.select("tailnum", "origin", "dest")

# Select the second set of columns
temp = flights.select(flights.origin, flights.dest, flights.carrier)

# Define first filter
filterA = flights.origin == "SEA"

# Define second filter
filterB = flights.dest == "PDX"

# Filter the data, first by filterA then by filterB
selected2 = temp.filter(filterA).filter(filterB)
```

#### More selection
- you can perform column operations with `.select()` and alias with `.alias()`
- you can do both operations with `selectExpr()`
    - the expression is passed in as a string

Example:
```python
# Define avg_speed
avg_speed = (flights.distance/(flights.air_time/60)).alias("avg_speed")

# Select the correct columns
speed1 = flights.select("origin", "dest", "tailnum", avg_speed)

# Create the same table using a SQL expression
speed2 = flights.selectExpr("origin", "dest", "tailnum", "distance/(air_time/60) as avg_speed")
```

### Aggregating
- You can do all the usual SQL aggregations with a pyspark `GroupedData` object
- use the `.groupBy()` method
- `df.groupBy().min("column").show()`

Example:
```python
# Find the shortest flight from PDX in terms of distance
flights.filter(flights.origin == "PDX").groupBy().min("distance").show()

# Find the longest flight from SEA in terms of air time
flights.filter(flights.origin == "SEA").groupBy().max("air_time").show()
```

Example2:
```python
# Average duration of Delta flights
flights.filter(flights.carrier == "DL").filter(flights.origin == "SEA").groupBy().avg("air_time").show()

# Total hours in the air
flights.withColumn("duration_hrs", flights.air_time/60).groupBy().sum("duration_hrs").show()
```