# Intro to API's

### Application Programming Interface
- standardized way for an application to communicate with another program
- no need to no about database details (schema, etc)
- good analogy, put in a well formed order to a store and get back what you asked for

### Python Requets
- `requests.get()` to get data from url
- `params` takes a dict of params and values to customize request
- `headers` takes a dict, can contain user authentication to API
- result is a `response` object
    - has a `.json()` method that will return just the JSON
    - returns a dictionary, but `read_json()` expects strings
    - trying to load dictionaries will produce an error

**Making a request for data**
```python
import requets
import pandas as pd

api_url = "https://api.yelp.com/v3/businesses/search"

params = {"term": "bookstore",
          "location": "San Francisco"}

headers = {"Authorization": "Bearer {}".format(api_key)}

response = requests.get(api_url,
                        params=params,
                        headers=headers)

data = response.json()
print(data)

bookstores = pd.DataFrame(data["businesses"])
print(bookstores.head(2))
```
### Working with Nested JSON
- JSON is nested when an object itself is nested
- `pandas.io.json` contains functions for reading and writing JSON
  - needs to be imported explicitly
  - use `json_normalize()`
    - takes a dictionary/list of dicts
    - returns a flattened data frame
    - defaults to naming convention: `attribute.nestedattribute`
    - you can choose a different separator with `sep` arg

**Example**
```python
import pandas as pd
import requests
from pandas.io.json import json_normalize

api_url = "https://api.yelp.com/v3/businesses/search"

headers = {"Authorization": "Bearer {}".format(api_key)}
params = {"term": "bookstore",
          "location": "San Francisco"}

response = requests.get(api_url,
                        headers=headers,
                        params=params)

data = response.json()

# flatten data and load to dataframe with separator specification
bookstores = json_normalize(data["businesses"], sep=",")
```

#### Deeply nested JSON
- `json_normalize()`
  - `record_path` string/list of string attributes to nested data
  - `meta` list of other attributes to load to data frame
  - `meta_prefix` string to prefix to meta column names

**Example**
```python
df = json_normalized(data["businesses"],
                     sep="_",
                     record_path="categories",
                     meta=["name",
                           "alias",
                           "rating",
                           ["coordinates", "latitude"],
                           ["coordinates", "longitude"]],
                     meta_prefix="biz_")
```
