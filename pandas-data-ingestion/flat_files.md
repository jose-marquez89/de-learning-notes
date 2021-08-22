# Flat Files
Pandas was originally introduced for financial quantitative analysis by Wes McKinney (2008).

### What's a flat file?
- one row per line separated by a delimiter

#### Loading
```python
import pandas as pd

df = pd.read_csv("somefile.csv", sep="defaultIsComma")
```

### Limiting columns
- use the `usecols` argument when reading things in
    - takes a list of column names or indices, even a function
- the `nrows` kwarg will limit things by a number of rows
    - you can use in combo with `skiprows` to process in chunks
    - if you skip the row with column names, specify `header=None`

### Assigning column names
- use the `names` kwarg to assign column names
- needs a name for each col
- when you print a dataframe as a list (`list(df)`) the resulting array will contain the column names
    - you could also use `df.columns`

### Specifying data types
- you can use `dtype` kwarg to specify a type
    - takes a dictionary to specify types: `dtype={"zipcode": str}`
- `df.dtypes` (plural) will give you the data types for each col in the df

### Custom missing data values
- use `na_values`
- lines that cause errors can be handle in the read-in phase
    - `error_bad_lines=False` skip records that can't be parsed
    - `warn_bad_lines=True` lets you see messages about skipped records
    - _it's worth investigating why a line caused an error_