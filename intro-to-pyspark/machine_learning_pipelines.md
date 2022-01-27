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