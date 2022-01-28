# Getting Started with Machine Learning Pipelines

## ML Pipelines
- `pyspark.ml` module
    - `Transformer` and `Estimator` classes
    - `Estimator`s implement a `.fit()` method
        - returns a model object

### Data types
- spark only handles numeric types in the ml context
    - int or double
- sometimes you need to cast a string to an int
    - use `.cast()` in combination with `.withColumn()`
    - `.cast()` takes a type as a string
    - `dataframe = dataframe.withColumn("col", dataframe.col.cast("new_type"))`

Example
```python
# Cast the columns to integers
model_data = model_data.withColumn("arr_delay", model_data.arr_delay.cast("integer"))
model_data = model_data.withColumn("air_time", model_data.air_time.cast("integer"))
model_data = model_data.withColumn("month", model_data.month.cast("integer"))
model_data = model_data.withColumn("plane_year", model_data.plane_year.cast("integer"))
```

### Feature Engineering
```python
# Create the column plane_age
model_data = model_data.withColumn("plane_age", model_data.year - model_data.plane_year)
```

#### Boolean columns
```python
# Create is_late
model_data = model_data.withColumn("is_late", model_data.arr_delay > 0)

# Convert to an integer
model_data = model_data.withColumn("label", model_data.is_late.cast("integer"))

# Remove missing values
model_data = model_data.filter("arr_delay is not NULL and dep_delay is not NULL and air_time is not NULL and plane_year is not NULL")
```

### Strings and Factors
You can one hot encode with features from `pyspark.ml.features`

How you can do this
- create a `StringIndexer`
- members of the class above are `Estimator`s that map each unique string in a column to a number
- the `Estimator` returns a `Transformer` that attaches mapping metadata to a dataframe and returns a new dataframe with a numeric column
- after you have the numeric column you can encode it with `OneHotEncoder`
    - this also creates an `Estimator` and then a `Transformer`
- the end result is a encoded vector

Example
```python
# Create a StringIndexer
carr_indexer = StringIndexer(inputCol="carrier", outputCol="carrier_index")

# Create a OneHotEncoder
carr_encoder = OneHotEncoder(inputCol="carrier_index", outputCol="carrier_fact")
```
Example2
```python
# Create a StringIndexer
dest_indexer = StringIndexer(inputCol="dest", outputCol="dest_index")

# Create a OneHotEncoder
dest_encoder = OneHotEncoder(inputCol="dest_index", outputCol="dest_fact")
```

Assembling features with the `VectorAssembler`
```python
# Make a VectorAssembler
vec_assembler = VectorAssembler(inputCols=["month", "air_time", "carrier_fact", "dest_fact", "plane_age"], outputCol="features")
```
The final step is to make your pipeline. This is similar to how we preprocess a dataset in pipeline fashion with scikit-learn
```python
# Import Pipeline
from pyspark.ml import Pipeline

# Make the pipeline
flights_pipe = Pipeline(stages=[dest_indexer, dest_encoder, carr_indexer, carr_encoder, vec_assembler])
```

### Splitting for Train and Test
```python
# Fit and transform the data
piped_data = flights_pipe.fit(model_data).transform(model_data)
```

Split the data into train and test
```python
# Split the data into training and test sets
training, test = piped_data.randomSplit([.6, .4])
```