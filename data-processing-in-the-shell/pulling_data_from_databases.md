# Pulling Data From Databases

### sql2csv
You can use `sql2csv` to pull data from sql sources

```bash
# --db for connection string
# do not use line breaks for the sql query here
sql2csv --db "sqlite:///SpotifyDatabase.db" \
        --query "SELECT * FROM Spotify_Popularity"
        > Spotify_Popularity.csv
```
_Note: if you don't redirect the output to a csv file, it will write the output to the console_

## Manipulating data using SQL syntax
You can use `csvsql` to use sql syntax on csv files. **Note**: It's a bit computationally expensive to use, be judicious here.

### Using csvsql
```bash
csvsql --query "SELECT * FROM Spotify_MusicAttributes LIMIT 1" \
    Spotify_MusicAttributes.csv

# format the output
csvsql --query "SELECT * FROM Spotify_MusicAttributes LIMIT 1" \
    data/Spotify_MusicAttributes.csv | csvlook
```

Joining CSV's with SQL syntax
```bash
# you need to write the query in one line
csvsql --query "SELECT * FROM file_a INNER JOIN file_b..." file_a.csv file_b.csv

# sometimes you may want to write the sql query in a shell variable first
sqlquery="SELECT * FROM file_a INNER JOIN file_b ON file_a.id = file_b.song_id"

csvsql --query "$sqlquery" Spotify_MusicAttributes.csv
```

**Random SQL Note**
Not sure what dialect you can do this in but apparently you can just immediately alias a table this way:
```SQL
SELECT ma.*, p.popularity 
FROM Spotify_MusicAttributes ma 
INNER JOIN Spotify_Popularity p 
ON ma.track_id = p.track_id
```