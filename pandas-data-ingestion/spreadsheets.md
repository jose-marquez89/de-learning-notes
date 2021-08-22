# Spreadsheets

### Using pandas
- `pd.read_excel('excelfile.xlsx`
- `read_excel` is very similar to `read_csv`

### Getting Data from multiple worksheets
- `sheet_name` argument to load in sheets other than the first one 
- `sheet_name` can take an iterable to bring in more sheets at time
- you can use (zero) index positions for `sheet_name`
- when you pass `None` to `sheet_name` it reads all the sheets
    - this will return a dictionary with
        - keys: sheetnames
        - values: dataframes corresponding to each sheet

If all the sheets have the same colums that describe the same data, you can join it all together:
```python
# get an empty df
all_responses = pd.DataFrame()

# loop thru resulting dict
for sheet_name, frame in survey_responses.items():
    # add an identifying col for years
    frame["Year"] = sheet_name

    # append to the empty frame
    all_responses = all_responses.append(frame)

print(all_responses.Year.unique())
```

### Modifying Imports