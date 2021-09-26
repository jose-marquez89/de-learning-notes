# Filtering data with csvkit

### Functions
- `csvcut`: filter using column name
- `csvgrep`: filter data by row (can pattern match with regex)

List column names
```
csvcut -n Spotify_MusicAttributes.csv
```
Return select columns
```bash
# return the first column
csvcut -c 1 Spotify_MusicAttribtes.csv

# alternatively
csvcut -c "track_id" Spotify_MusicAttributes.csv

# return the second and third column
csvcut -c 1,2 Spotify_MusicAttributes.csv

# return multiple columns by name
csvcut -c "danceability", "duration_ms" Spotify_MusicAttributes.csv
```

### Matching rows with csvgrep
Use `csvgrep` with options
- exact match or regex fuzzy matching
- uses options below
    - `-m`: followed by exact row val
    - `-r`: followed by regex pattern
    - `-f`: followed by path to file

Find things in a file where `track_id` = `some_id`
```bash
# don't use quotations for the row value
csvgrep -c "some_colname" -m some_row_val filename.csv

# alternatively
csvgrep -c 1 -m some_row_val filename.csv
```
