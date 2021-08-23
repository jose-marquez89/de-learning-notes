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
- sometimes N/A values will be coded as true when cast to bool
- `read_excel()` has a `true_values` and a `false_values` argument
    - these can set custom values to be interpreted as true/false
- considerations
    - are there (or could there be in the future) missing values?
    - how will the column be used in analysis?
    - what would happen if a value was incorrectly coded as true
    - could the data be modeled another way (floats, integers, etc)

_Setting true and false values (dtypes as a dict)_
```python
# Load file with Yes as a True value and No as a False value
survey_subset = pd.read_excel("fcc_survey_yn_data.xlsx",
                              dtype={"HasDebt": bool,
                              "AttendedBootCampYesNo": bool},
                              true_values=["Yes"],
                              false_values=["No"])

# View the data
print(survey_subset.head())
```

#### Parsing Dates