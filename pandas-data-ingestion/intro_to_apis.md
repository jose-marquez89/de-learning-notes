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