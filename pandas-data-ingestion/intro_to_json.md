# Intro to Json

### JSON
- common web data format
- not tabular
  - records don't all have to have the same attributes
- objects are collections of attribute-value pairs
- objects within objects

### Reading Json
- `read_json()`
- `dtype` arg for types
- pandas guesses how to arrange it into a table
- pandas can automatically handle some common orientations

#### Specifying Orientation
```python
import pandas as pd
death_cause = pd.read_json("nyc_death_causes.json",
                           orient="split")