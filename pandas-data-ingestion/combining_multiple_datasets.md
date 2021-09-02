# Combining

### Appending
- `.append()`
    - `df1.append(df2)`
    - set `ignore_index` to true to renumber rows

### Merging
- `.merge()` is both a pandas dataframe and function 
    - use kwarg `on` if key names are the same
    - use kwargs `left_on` and `right_on` if columns have different names